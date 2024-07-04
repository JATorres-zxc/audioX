<template>
    <main class="flex flex-col h-screen">
      <!-- Upper div for music player -->
      <div class="flex border border-red-500 m-1 justify-center items-center">
        <audio ref="audioPlayer" src="../assets/NVMD.mp3"></audio>
      </div>
  
      <!-- Lower div with placeholders -->
      <div class="flex flex-1">
        <div class="flex-1 border border-red-500 m-1"></div>
        <div class="flex-1 border border-green-500 m-1"></div>
        <div class="flex-1 border border-yellow-500 m-1"></div>
      </div>
    </main>
  </template>
  
  <script>
  export default {
    mounted() {
      // Example: Auto-play the audio when component is mounted
      this.$refs.audioPlayer.play();
    }
  };
  </script>

  


