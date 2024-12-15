<template>
  <div class="tts-reader-container">
    <h3>TTS Reader</h3>
    <textarea v-model="text" rows="15" cols="80"  placeholder="Enter text here..."></textarea>
    <br />
    <audio controls ref="audio">
      <source :src="audioSource" type="audio/wav" />
    </audio>

    <br />
    <transition name="fade">
      <button @click="playAudio" :class="{ 'animated-button': isPlaying }">PLAY</button>
    </transition>
    <br />
    <div class="character-count">Number of Characters: <span>{{ text.length }}</span></div>
    <br />
    <hr />
  </div>
</template>
  
  <script>
export default {
  data() {
    return {
      text: '',
      audioSource: '',
      isPlaying: false,
    };
  },
    methods: {
      async playAudio() {
        if (this.text === '') {
          alert('You must enter some text...');
          return;
        }
  
        try {
          this.isPlaying = true;
  
          let speak = await fetch('https://api.streamelements.com/kappa/v2/speech?voice=Brian&text=' + encodeURIComponent(this.text.trim()));
  
          if (!speak.ok) {
            const errorData = await speak.json();
            alert(errorData.message || 'An error occurred.');
            return;
          }
  
          let mp3 = await speak.blob();
          this.audioSource = URL.createObjectURL(mp3);
  
          let audio = this.$refs.audio;
          audio.pause();
          audio.load();
          audio.play();
  
          
          audio.addEventListener('ended', () => {
            this.isPlaying = false;
          });
        } catch (error) {
          console.error(error);
          this.isPlaying = false;
        }
      },
    },
    watch: {
    isPlaying(value) {
      if (value) {
        this.isDarkTheme = true;
      } else {
        this.isDarkTheme = false;
      }
    },
  },
};
</script>
  
  <style scoped>
  .tts-reader-container {
    max-width: 600px;
    margin: auto;
    padding: 20px;
    text-align: center;
  }
  
  textarea {
    margin-top: 10px;
  }
  
  button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }
  
  .animated-button {
    animation: pulse 1s infinite;
  }
  
  .character-count {
    margin-top: 10px;
  }
  
  
  @keyframes pulse {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.1);
    }
    100% {
      transform: scale(1);
    }
  }
  </style>
  
