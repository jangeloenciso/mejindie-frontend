<template>
    <div>
      <h1>{{ title }}</h1>
      <video ref="video" class="custom-video-player" width="640" controls></video>
    </div>
  </template>
  
  <script>
  export default {
    name: 'VideoPlayer',
    props: {
      fileName: String,
      title: String
    },
    mounted() {
      // Dynamically load Shaka Player
      const script = document.createElement('script');
      script.src = 'https://cdnjs.cloudflare.com/ajax/libs/shaka-player/3.0.0/shaka-player.compiled.js';
      script.onload = this.initializePlayer;
      document.head.appendChild(script);
    },
    methods: {
      initializePlayer() {
        // Use window.shaka to access Shaka Player
        const video = this.$refs.video;
        const player = new window.shaka.Player(video);
        window.player = player;  // Attach player to window for debugging
  
        // Load the DRM-protected content
        player.load(`http://192.168.1.7:3000/uploads/${this.fileName}.mp4`).then(() => {
          console.log('The video has been loaded!');
        }).catch(error => {
          console.error('Error loading video:', error);
        });
      }
    }
  }
  </script>
  
  <style>
  /* Container for centering and spacing */
  .video-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    background-color: #f8f8f8;
  }
  
  /* Title styling */
  .video-container h1 {
    font-size: 24px;
    color: #333;
    margin-bottom: 15px;
  }
  
  /* Customized video player styling */
  .custom-video-player {
    width: 100%; /* Make the video responsive */
    max-width: 1080px;
    border: 3px solid #444;
    border-radius: 10px;
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.2);
  }
  
  /* Adjust control styling */
  .custom-video-player::-webkit-media-controls-panel {
    background-color: rgba(0, 0, 0, 0.7); /* Darken control background */
  }
  
  .custom-video-player::-webkit-media-controls-play-button,
  .custom-video-player::-webkit-media-controls-timeline,
  .custom-video-player::-webkit-media-controls-volume-slider {
    filter: hue-rotate(10deg); /* Adjust color of controls */
  }
  </style>