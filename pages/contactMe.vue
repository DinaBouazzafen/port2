<template>
  <div>
    <cursor />
    
    <div class="contact-me">
      <div class="drawer relative z-10 pr-5">
          <Drawer />
      </div>

      <!-- Contact Header -->
      <div class="contact-header">
        <img src="/img/contact-me.png" alt="Contact Me" class="title-img" />
        <p class="contact-intro hnmi">Always welcome to send a mail or to befriend me on insta!</p>
      </div>

      <!-- Contact Buttons -->
      <div class="contact-buttons">
        <a 
          href="mailto:dina.bouazzafen@student.luca-arts.be" 
          target="_blank"
          rel="noopener noreferrer"
          class="contact-button"
        >
          <img src="/img/buttons/button-email.png" alt="Email Button" class="button-img" />
        </a>
        <a
          href="https://www.instagram.com/dina_buckhain/"
          target="_blank"
          rel="noopener noreferrer"
          class="contact-button"
        >
          <img src="/img/buttons/button-instagram.png" alt="Instagram Button" class="button-img" />
        </a>
      </div>

      <!-- Loader GIF -->
      <div class="loader-container">
        <img src="/img/loader.gif" alt="Logo" class="loader-gif" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
name: "ContactMe",
};
</script>

<style scoped>



.contact-me {
position: relative;
min-height: 100vh;
overflow: hidden;
}

.cursor {
z-index: 1000;
}

/* Header Styling */
.contact-header {
margin: 60px auto;
text-align: center;
}

.contact-header .title-img {
width: 100%; /* Full width */
max-height: 200px; /* Restrict height for better visuals */
object-fit: contain; /* Ensure proper aspect ratio */
margin-bottom: 20px;
filter: brightness(200%);
mix-blend-mode: difference;

}

.contact-header .contact-intro {
font-size: 1.5rem;
margin-bottom: 50px;
}

/* Contact Buttons */
.contact-buttons {
display: flex; /* Align buttons horizontally */
flex-direction: row; /* Row layout for horizontal alignment */
justify-content: center; /* Center buttons */
gap: 40px; /* Space between buttons */
}

.contact-button .button-img {
width: 600px; /* Larger button width */
height: auto;
transition: transform 0.3s ease-in-out, filter 0.3s ease-in-out;
filter: brightness(150%);
mix-blend-mode: difference;

}

.contact-button .button-img:hover {
transform: scale(1.2); /* Larger hover effect */
filter: brightness(150%);
}

/* Loader GIF Styling */
.loader-container {
  display: flex; /* Use flexbox */
  justify-content: center; /* Horizontally center the content */
  align-items: center; /* Vertically center the content if needed */
  margin-top: 10px; /* Add space between buttons and GIF */
}

.loader-gif {
width: 500px; /* Adjust size of the GIF */
height: auto;
filter: brightness(150%);
mix-blend-mode: difference;

}
</style>