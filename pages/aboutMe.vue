<template>
    <div>
      <cursor />
    </div>
    
    <div class="about-me">
      <!-- Fullscreen video -->
      <div class="video-container">
        <iframe
          title="vimeo-player"
          src="https://www.youtube.com/embed/Ebnbi-8eLi0?autoplay=1&mute=1&loop=1&playlist=Ebnbi-8eLi0"
          frameborder="0"
          allowfullscreen
          allow="autoplay"
        ></iframe>
        <!-- Add a button or event trigger for scrolling -->
        <button class="scroll-button hnmi" @click="scrollToDrawer">About me</button>
      </div>
  
      <!-- Drawer Section -->
      <div class="drawer relative z-10 pr-5">
        <Drawer />
      </div>
  
      <!-- Title and Cards -->
      <div class="aboutmetitle">
        <img id="title" src="/img/about-me.png" />
      </div>
      <div class="aboutme-container">
        <div
          class="card"
          @mouseover="flipCard('me')"
          @mouseleave="unflipCard('me')"
        >
          <div class="card-inner" :class="{ flipped: flippedCards.me }">
            <div class="card-front">
              <img src="/cards/me.png" />
            </div>
            <div class="card-back">
              <img src="/cards/me-back.png" />
            </div>
          </div>
        </div>
        <div
          class="card"
          @mouseover="flipCard('hobbies')"
          @mouseleave="unflipCard('hobbies')"
        >
          <div class="card-inner" :class="{ flipped: flippedCards.hobbies }">
            <div class="card-front">
              <img src="/cards/hobbies.png" />
            </div>
            <div class="card-back">
              <img src="/cards/hobbies-back.png" />
            </div>
          </div>
        </div>
        <div
          class="card"
          @mouseover="flipCard('skills')"
          @mouseleave="unflipCard('skills')"
        >
          <div class="card-inner" :class="{ flipped: flippedCards.skills }">
            <div class="card-front">
              <img src="/cards/skills.png" />
            </div>
            <div class="card-back">
              <img src="/cards/skills-back.png" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
export default {
  data() {
    return {
      flippedCards: {
        me: false,
        hobbies: false,
        skills: false,
      },
    };
  },
  methods: {
    flipCard(card) {
      this.flippedCards[card] = true;
    },
    unflipCard(card) {
      this.flippedCards[card] = false;
    },
    scrollToDrawer() {
      const drawerElement = document.querySelector(".drawer");
      drawerElement.scrollIntoView({ behavior: "smooth" });
    },
  },
};
  </script>
  
  <style scoped>
  .about-me {
    scroll-snap-type: y mandatory; /* Enable snap scrolling along the vertical axis */
    overflow-y: scroll; /* Allow vertical scrolling */
    scroll-behavior: smooth;
    top: 0;  
    left: 0;  
    width: 100%;  
    height: 100%;
  }
  .scroll-button {
    position: absolute;
    bottom: 10px;
    left: 50%;
    transform: translateX(-50%);
    padding: 10px 20px;
    background: url('/img/buttons/button-aboutMe.png') no-repeat center center;
    background-size: cover; /* Ensure the image covers the entire button area */
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    color: transparent; /* Hide text, as the image will serve as the button content */
    width: 270px; /* Adjust the width based on the image dimensions */
    height: 60px; /* Adjust the height based on the image dimensions */
    z-index: 100;
    transition: transform 0.2s;
    filter: brightness(150%);

  }
  
  .scroll-button:hover {
    transform: translateX(-50%) scale(1.1);
  }
  .video-container {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden; /* Prevents scrollbars if the iframe overflows */
    mix-blend-mode: difference;
    scroll-snap-align: start; 
  }
  
  .video-container iframe {
    position: absolute;
    top: 50%;
    left: 50%;
    min-width: 100vw; /* Ensures the video stretches to cover the full width */
    min-height: 100vh; /* Ensures the video stretches to cover the full height */
    width: auto; /* Allows the iframe to scale proportionally */
    height: auto; /* Ensures scaling respects the aspect ratio */
    transform: translate(-50%, -50%);
    object-fit: cover; /* Ensures no blank spaces in the canvas */
    border: none; /* Removes any border on the iframe */
    pointer-events: none;
  }
  .drawer{
    position: relative;
    z-index: 10;
    padding-right: 5px;
    scroll-snap-align: start; /* Snap this section to the top of the viewport */
  }
  .aboutmetitle {
    margin: 20px 0;
    margin-top: -10px;
    margin-bottom: 50px;
    display: flex;
    justify-content: center;
    position: relative;
    align-items: center;
    filter: brightness(300%);
    mix-blend-mode:normal;
    mix-blend-mode: difference;

  }
  
  .aboutme-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    padding: 10px;
    justify-items: center;
    mix-blend-mode:difference;
    filter: brightness(150%);

  }
  
  .card {
    position: relative;
    width: 300px;
    height: 550px;
    perspective: 1000px;
  }
  
  .card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    transform-style: preserve-3d;
    transition: transform 0.6s;
  }
  
  .card-inner.flipped {
    transform: rotateY(180deg);
  }
  
  .card-front,
  .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius: 10px;
  }
  
  .card-front {
    transform: rotateY(0deg);
  }
  
  .card-back {
    transform: rotateY(180deg);
  }
  </style>
  