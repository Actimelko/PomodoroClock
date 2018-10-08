<template>
  <div id="app" class="container">
    <h1 class="title is-1 has-text-centered">Pomodoro Clock</h1>
    <div class="box controls">

      <break-block 
        :break-length="clock.breakLength"
        @update="clock.updateBreakLength($event)" />

      <session-block 
        :session-length="clock.sessionLength"
        @update="clock.updateSessionLength($event)" />
    </div>
    
    <display-block 
      :timer="getTimer"
      @start="clock.handlerDisplay($event)"/>

    <audio src="src/audio/clock_town_music_box.mp3" id="audio"></audio>
  </div>
</template>

<script>
import BreakBlock from './components/BreakBlock.vue';
import SessionBlock from './components/SessionBlock.vue';
import DisplayBlock from './components/DisplayBlock.vue';

class Clock {
  constructor(options) {
    this.breakLength = 5;
    this.sessionLength = 25;
    this.timer = {
      minutes: this.sessionLength,
      seconds: 0
    };
    this.isActive = false;
    this.audio = null;
  }

  updateBreakLength(name) {
    if (name == 'plus') this.plusBreakLength();
    if (name == 'minus') this.minusBreakLength();
  }

  plusBreakLength() {
    if (this.breakLength === 15) return;
    ++this.breakLength;
  }

  minusBreakLength() {
    if (this.breakLength === 0) return;
    --this.breakLength;
  }

  updateSessionLength(name) {
    if (name == 'plus') this.plusSessionLength();
    if (name == 'minus') this.minusSessionLength();
  }

  plusSessionLength() {
    if (this.sessionLength === 60) return;
    ++this.sessionLength;
    this.timer.minutes = this.sessionLength;
    this.timer.seconds = 0;
  }

  minusSessionLength() {
    if (this.sessionLength === 0) return;
    --this.sessionLength;
    this.timer.minutes = this.sessionLength;
    this.timer.seconds = 0;
  }

  handlerDisplay(name) {
    if (name === 'start') this.start();
    if (name === 'pause') this.pause();
    if (name === 'refresh') this.refresh();
  }

  playAudio() { 
    this.audio = document.getElementById('audio');
    this.audio.play();
  }

  refresh() {
    this.isActive = false;
    this.breakLength = 5;
    this.sessionLength = 25;
    this.timer.minutes = this.sessionLength;
    this.timer.seconds = 0;
    clearInterval(this.timer.timer);
  }

  start() {
    if (this.isActive) return;
    if (this.timer.seconds === 0 && this.timer.minutes === 0) return;
    this.isActive = true;
    this.timer.timer = setInterval(()=>{
      if (this.timer.seconds === 0 ) {
        this.timer.seconds = 60;
        --this.timer.minutes; 
      }
      --this.timer.seconds;
      if (this.timer.seconds === 0 && this.timer.minutes === 0) {
        clearInterval(this.timer.timer);
        this.playAudio();
      }
    },1000)
  }

  pause() {
    if (!this.isActive) return;
    this.isActive = false;
    clearInterval(this.timer.timer);
  }

  viewMinutes() {
    const formMinutes = (this.timer.minutes < 10 ? 
      '0' + this.timer.minutes : this.timer.minutes);
    return formMinutes;
  }

  viewSeconds() {
    const formSeconds = (this.timer.seconds < 10 ? 
      '0' + this.timer.seconds : this.timer.seconds);
    return formSeconds; 
  }
}

const vm = {
  name: 'app',
  components: {
    BreakBlock, SessionBlock, DisplayBlock
  },
  data() {
     return {
       clock: new Clock()
     }
  },
  computed: {
    getTimer() {
      return this.clock.viewMinutes() + ":" + this.clock.viewSeconds();
    }
  },
  methods: {
    
  }
}

export default vm;
</script>

<style>
  .controls {
    display: flex;
    justify-content: space-around;
    align-items: baseline;
  }
</style>
