<template>
  <div class="block">
    <div
      class="block-container block-beforeStart"
      @click="startGame"
      ref="beforeStart"
      v-if="!this.isPlaying"
    >
      <h2 class="block-title">Click to start game</h2>
    </div>
    <div
      class="block-container block-gameWindow"
      @click="this.state === this.GAME_STATES.PLAYING ? gameLogic() : restart()"
      ref="gameWindow"
      v-else-if="isPlaying"
    >
      <h2 class="block-title" v-if="this.state === this.GAME_STATES.PLAYING">
        Wait for time
      </h2>
      <div class="block-time" v-if="this.state === this.GAME_STATES.DONE">
        <h2 class="block-time">
          {{
            this.startTime
              ? `Your Reaction Time is ${this.reactionTime}ms`
              : "You Clicked Early"
          }}
        </h2>
        <p class="block-command">Click Again To Restart</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    let currentDelay = ref(null),
      isPlaying = ref(false);
    const GAME_STATES = {
      NEW_GAME: 0,
      PLAYING: 1,
      DONE: 2,
    };
    let reactionTime = ref(Infinity);
    let startTime = ref(0);

    let state = ref(GAME_STATES.NEW_GAME);
    let beforeStart = ref(null),
      gameWindow = ref(null);

    return {
      state,
      GAME_STATES,
      currentDelay,
      isPlaying,
      beforeStart,
      gameWindow,
      reactionTime,
      startTime,
    };
  },

  methods: {
    startGame() {
      this.currentDelay = this.createDelay();
      this.isPlaying = true;
      this.startTime = 0;
      this.state = this.GAME_STATES.PLAYING;

      this.timeout = setTimeout(() => {
        this.startTime = Date.now();
        this.gameWindow.classList.add("clickNow");
      }, this.currentDelay);
    },
    createDelay() {
      return 2000 + Math.ceil(Math.random() * 3000);
    },
    gameLogic() {
      // this.isPlaying = false;

      this.state = this.GAME_STATES.DONE;
      this.gameWindow.classList.remove("clickNow");

      if (this.startTime === 0) {
        this.gameWindow.classList.add("fail");
      } else {
        this.gameWindow.classList.add("success");
        this.reactionTime = Date.now() - this.startTime;
      }
      clearTimeout(this.timeout);
      console.log(this.startTime, this.reactionTime);
      console.log(this.state);
    },
    restart() {
      this.gameWindow.classList.remove("fail");
      this.gameWindow.classList.remove("success");
      this.startGame();
    },
  },
};
</script>

<style>
.block {
  height: 400px;
  color: #222;
}
.block-container {
  display: grid;
  place-content: center;

  height: 100%;
}
.block-beforeStart {
  background: #00f0ee;
}
.block-gameWindow {
  background: hsl(54, 100%, 50%);
}
.block-gameWindow.success {
  background: #0f0;
}
.block-gameWindow.fail {
  background: #f00;
}
.block-gameWindow.clickNow {
  background: #00f;
  color: #eee;
}
</style>