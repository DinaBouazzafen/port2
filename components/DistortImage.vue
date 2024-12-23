<script>
import { onMounted, ref, onBeforeUnmount } from 'vue'
import { Application, Assets, Sprite, BaseTexture, Texture } from 'pixi.js'
import { DisplacementFilter } from '@pixi/filter-displacement'
import { WRAP_MODES } from '@pixi/constants'

export default {
  props: {
    imageSrc: {
      type: String,
      required: true
    },
    displacementSrc: {
      type: String,
      required: true
    },
    width: {
      type: Number,
      default: 600
    },
    height: {
      type: Number,
      default: 400
    },
    displacementScale: {
      type: Number,
      default: 100
    }
  },
  setup(props) {
    const container = ref(null)
    let app, imageSprite, displacementSprite, displacementFilter, handleMouseMove

    onMounted(async () => {
      if (typeof window === 'undefined') return

      app = new Application()
      await app.init({
        width: props.width,
        height: props.height,
        backgroundAlpha: 0
      })

      container.value.appendChild(app.canvas)

      const [imageTexture, displacementResource] = await Promise.all([
        Assets.load(props.imageSrc),
        Assets.load(props.displacementSrc)
      ])

      // imageTexture is a Texture
      // displacementResource is also a Texture loaded by Assets

      // Create the main image sprite
      imageSprite = new Sprite(imageTexture)
      imageSprite.width = props.width
      imageSprite.height = props.height
      app.stage.addChild(imageSprite)

      // Create a BaseTexture from the displacement resource's URL
      // and set addressMode (replaces wrapMode)
      const displacementBaseTexture = BaseTexture.from(props.displacementSrc)
      displacementBaseTexture.addressMode = WRAP_MODES.REPEAT

      // Create a Texture from the BaseTexture
      const displacementTexture = new Texture(displacementBaseTexture)

      displacementSprite = new Sprite(displacementTexture)
      displacementSprite.scale.set(2)

      displacementFilter = new DisplacementFilter(displacementSprite)
      displacementFilter.scale.set(0, 0)
      app.stage.addChild(displacementSprite)
      app.stage.filters = [displacementFilter]

      // Mouse movement handler
      handleMouseMove = (e) => {
        const rect = app.canvas.getBoundingClientRect()
        const mouseX = e.clientX - rect.left
        const mouseY = e.clientY - rect.top

        displacementSprite.x = mouseX
        displacementSprite.y = mouseY

        displacementFilter.scale.x = (mouseX - props.width / 2) * (props.displacementScale / props.width)
        displacementFilter.scale.y = (mouseY - props.height / 2) * (props.displacementScale / props.height)
      }

      // Event listeners
      app.canvas.addEventListener('mousemove', handleMouseMove)
      app.canvas.addEventListener('mouseleave', () => {
        displacementFilter.scale.set(0, 0)
      })
    })

    onBeforeUnmount(() => {
      if (app) {
        app.destroy(true, true)
      }
    })

    return {
      container
    }
  }
}
</script>

<template>
  <div ref="container" class="distort-container"></div>
</template>

<style scoped>
.distort-container {
  display: inline-block;
  cursor: pointer;
  overflow: hidden;
}
</style>
