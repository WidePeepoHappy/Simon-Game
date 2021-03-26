<template>
  <div class="container">
    <div id="main-circle">
      <div
        @[playerClick]="playerMove('color1')"
        :style="{ backgroundColor: color1 }"
        id="topleft"
      ></div>
      <div
        @[playerClick]="playerMove('color2')"
        :style="{ backgroundColor: color2 }"
        id="topright"
      ></div>
      <div
        @[playerClick]="playerMove('color3')"
        :style="{ backgroundColor: color3 }"
        id="bottomleft"
      ></div>
      <div
        @[playerClick]="playerMove('color4')"
        :style="{ backgroundColor: color4 }"
        id="bottomright"
      ></div>
    </div>
    <button @[playerCanReStart]="startGame" class="button" id="start">
      Старт
    </button>
    <h2>Раунд: {{ round }}</h2>
    <h2 v-if="lost">
      Вы проиграли после {{ round }} {{ round === 1 ? "раунда" : "раундов" }}
    </h2>
  </div>
</template>

<script>
export default {
  props: ["mode"],
  data() {
    return {
      color1: "darkgreen",
      color2: "darkred",
      color3: "goldenrod",
      color4: "darkblue",
      sequence: [],
      playerSequence: [],
      playerTurn: false,
      canReStart: true,
      lost: false,
      round: 0,
      intervalId: null,
    };
  },
  computed: {
    playerClick() {
      return this.playerTurn ? "click" : null;
    },
    playerCanReStart() {
      return this.canReStart ? "click" : null;
    },
  },
  methods: {
    startGame() {
      this.sequence = [];
      this.playerSequence = [];
      this.round = 1;
      this.lost = false;
      this.playerTurn = false;
      this.newRound();
    },
    newRound() {
      this.canReStart = false;
      this.sequence.push(Math.floor(Math.random() * 4) + 1);
      this.initSequence(this.sequence);
    },
    initSequence(sequence) {
      let i = 0;
      this.intervalId = setInterval(() => {
        this.clearColor();

        if (i >= sequence.length) {
          clearInterval(this.intervalId);
          this.playerTurn = true;
          this.canReStart = true;
        } else {
          setTimeout(() => {
            this.flashTile(sequence[i]);
            i++;
          }, 200);
        }
      }, this.checkMode());
    },
    checkMode() {
      return this.mode === "easy" ? 1500 : this.mode === "medium" ? 1000 : 400;
    },
    playerMove(color) {
      if (color === "color1") {
        this.playerSequence.push(1);
        this.check();
        this.flashTile(1);
        setTimeout(() => {
          this.clearColor();
        }, 300);
      } else if (color === "color2") {
        this.playerSequence.push(2);
        this.check();
        this.flashTile(2);
        setTimeout(() => {
          this.clearColor();
        }, 300);
      } else if (color === "color3") {
        this.playerSequence.push(3);
        this.check();
        this.flashTile(3);
        setTimeout(() => {
          this.clearColor();
        }, 300);
      } else if (color === "color4") {
        this.playerSequence.push(4);
        this.check();
        this.flashTile(4);
        setTimeout(() => {
          this.clearColor();
        }, 300);
      }
    },
    check() {
      if (
        this.playerSequence[this.playerSequence.length - 1] !==
        this.sequence[this.playerSequence.length - 1]
      ) {
        this.lost = true;
        this.clearColor();
        this.playerTurn = false;
      }
      if (this.round == this.playerSequence.length && !this.lost) {
        this.round++;
        this.playerSequence = [];
        this.playerTurn = false;
        this.newRound();
      }
    },
    flashTile(tile) {
      if (tile === 1) {
        this.color1 = "lightgreen";
        let audio = document.getElementById("clip1");
        audio.currentTime = 0;
        audio.play();
      } else if (tile === 2) {
        this.color2 = "tomato";
        let audio = document.getElementById("clip2");
        audio.currentTime = 0;
        audio.play();
      } else if (tile === 3) {
        this.color3 = "yellow";
        let audio = document.getElementById("clip3");
        audio.currentTime = 0;
        audio.play();
      } else if (tile === 4) {
        this.color4 = "lightskyblue";
        let audio = document.getElementById("clip4");
        audio.currentTime = 0;
        audio.play();
      }
    },
    clearColor() {
      this.color1 = "darkgreen";
      this.color2 = "darkred";
      this.color3 = "goldenrod";
      this.color4 = "darkblue";
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
}
#start {
  font-size: 25px;
  background-color: lightblue;
  margin-top: 20px;
}

#main-circle {
  background: #385a94;
  border-radius: 50%;
  height: 300px;
  width: 300px;
  position: relative;
  border-style: solid;
  border-width: 5px;
  margin-right: 10px;
}

#topleft {
  position: absolute;
  height: 150px;
  width: 150px;
  border-radius: 150px 0 0 0;
  -moz-border-radius: 150px 0 0 0;
  -webkit-border-radius: 150px 0 0 0;
  background: darkgreen;
  top: 50%;
  left: 50%;
  margin: -150px 0px 0px -150px;
  border-style: solid;
  border-width: 2.5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}

#topright {
  position: absolute;
  display: inline-block;
  height: 150px;
  width: 150px;
  border-radius: 0 150px 0 0;
  -moz-border-radius: 0 150px 0 0;
  -webkit-border-radius: 0 150px 0 0;
  background: darkred;
  top: 50%;
  left: 50%;
  margin: -150px 0px 0px 0px;
  border-style: solid;
  border-width: 2.5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}

#bottomleft {
  position: absolute;
  height: 150px;
  width: 150px;
  border-radius: 0 0 0 150px;
  -moz-border-radius: 0 0 0 150px;
  -webkit-border-radius: 0 0 0 150px;
  background: goldenrod;
  top: 50%;
  left: 50%;
  margin: 0px -150px 0px -150px;
  border-style: solid;
  border-width: 2.5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}

#bottomright {
  position: absolute;
  height: 150px;
  width: 150px;
  border-radius: 0 0 150px 0;
  -moz-border-radius: 0 0 150px 0;
  -webkit-border-radius: 0 0 150px 0;
  background: darkblue;
  top: 50%;
  left: 50%;
  margin: 0px 0px -150px 0px;
  border-style: solid;
  border-width: 2.5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
</style>