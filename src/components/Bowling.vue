<script setup>
import { ref, onMounted } from "vue";

const mask = ref(null);
const hint = ref(null);
const bowlingBalls = ref([]);
const pins = ref([]);
const character = ref(null);
const characterBall = ref(null);
const startBtn = ref(null);

/**
 * Move character image to center
 */
const initialMovie = () => {
  const characterImage = character.value.animate([{ marginTop: "-10%" }], {
    duration: 1000,
    iterations: 1,
  });
  const characterBallImage = characterBall.value.animate([{ marginTop: "0%" }], {
    duration: 1000,
    iterations: 1,
  });
  setTimeout(() => {
    characterImage.pause();
    characterBallImage.pause();
    startBtn.value.style.display = "inline-block";
  }, 900);
};

/**
 * Start game
 */
const start = () => {
  mask.value.style.display = "none";
  hint.value.style.display = "block";
  bowlingBalls.value[0].style.display = "inline-block";
  bowlingBalls.value[1].style.display = "inline-block";
  bowlingBalls.value[2].style.display = "inline-block";
};

/**
 * Choose bowling ball
 */
const chooseBall = (ballNo) => {
  hint.value.style.display = "none";
  bowlingBalls.value[0].style.display = "none";
  bowlingBalls.value[1].style.display = "none";
  bowlingBalls.value[2].style.display = "none";
  bowlingBalls.value[ballNo].style.display = "inline-block";
  let timer = null;
  setTimeout(() => {
    bowlingBalls.value[ballNo].style.animationPlayState = "running";
    timer = setInterval(() => {
      if (
        bowlingBalls.value[ballNo].getBoundingClientRect().top <=
        pins.value[0].getBoundingClientRect().bottom
      ) {
        hit(ballNo);
        clearInterval(timer);
      }
    });
    setTimeout(() => {
      bowlingBalls.value[ballNo].style.animationPlayState = "paused";
    }, 2900);
  }, 100);
};

/**
 * Start pins random runway
 *
 * @param {number} index
 *
 * @returns
 */
const randomPinRun = (index) => {
  let plusMinus = Math.ceil(Math.random() * 100) >= 50 ? "" : "-";
  let plusMinus2 = Math.ceil(Math.random() * 100) >= 50 ? "" : "-";
  let translateX = plusMinus + Math.ceil(Math.random() * 361);
  let translateY = plusMinus2 + Math.ceil(Math.random() * 361);
  let rotate = plusMinus + Math.ceil(Math.random() * 361);
  return pins.value[index].animate(
    [
      { transform: "translate(0) rotate(0)" },
      {
        transform:
          "translate(" +
          translateX +
          "px, " +
          translateY +
          "px) rotate(" +
          rotate +
          "deg)",
      },
    ],
    {
      duration: 1000,
      easing: "ease-out",
      fill: "both",
      iterations: 1,
    }
  );
};

/**
 * Ball hit pins event
 */
const hit = (ballNo) => {
  const pin1 = randomPinRun(0);
  setTimeout(() => {
    pin1.pause();
  }, 900);

  setTimeout(() => {
    const pin2 = randomPinRun(1);
    const pin3 = randomPinRun(2);
    setTimeout(() => {
      pin2.pause();
      pin3.pause();
    }, 900);
  }, 100);

  setTimeout(() => {
    const pin4 = randomPinRun(3);
    const pin5 = randomPinRun(4);
    const pin6 = randomPinRun(5);
    setTimeout(() => {
      pin4.pause();
      pin5.pause();
      pin6.pause();
    }, 900);
  }, 300);

  setTimeout(() => {
    bowlingBalls.value[ballNo].style.display = "none";
    const pin7 = randomPinRun(6);
    const pin8 = randomPinRun(7);
    const pin9 = randomPinRun(8);
    const pin10 = randomPinRun(9);
    setTimeout(() => {
      pin7.pause();
      pin8.pause();
      pin9.pause();
      pin10.pause();
    }, 900);
  }, 400);
};

onMounted(() => {
  initialMovie();
});
</script>

<template>
  <div class="content">
    <div>
      <div id="game">
        <div class="lane cell">
          <div style="display: flex; margin-left: 30%; padding-top: 47%">
            <img
              id="pin10"
              :ref="(el) => (pins[9] = el)"
              class="pin_line4"
              src="../assets/pin.png"
            />
            <img
              id="pin9"
              :ref="(el) => (pins[8] = el)"
              class="pin_line4"
              src="../assets/pin.png"
            />
            <img
              id="pin8"
              :ref="(el) => (pins[7] = el)"
              class="pin_line4"
              src="../assets/pin.png"
            />
            <img
              id="pin7"
              :ref="(el) => (pins[6] = el)"
              class="pin_line4"
              src="../assets/pin.png"
            />
          </div>
          <div style="display: flex; margin-left: 34%; margin-top: -8%">
            <img
              id="pin6"
              :ref="(el) => (pins[5] = el)"
              class="pin_line3"
              src="../assets/pin.png"
            />
            <img
              id="pin5"
              :ref="(el) => (pins[4] = el)"
              class="pin_line3"
              src="../assets/pin.png"
            />
            <img
              id="pin4"
              :ref="(el) => (pins[3] = el)"
              class="pin_line3"
              src="../assets/pin.png"
            />
          </div>
          <div style="display: flex; margin-left: 39%; margin-top: -8%">
            <img
              id="pin3"
              :ref="(el) => (pins[2] = el)"
              class="pin_line2"
              src="../assets/pin.png"
            />
            <img
              id="pin2"
              :ref="(el) => (pins[1] = el)"
              class="pin_line2"
              src="../assets/pin.png"
            />
          </div>
          <div style="display: flex; margin-left: 45%; margin-top: -8%">
            <img
              id="pin1"
              :ref="(el) => (pins[0] = el)"
              class="pin_line1"
              src="../assets/pin.png"
            />
          </div>
          <div id="mask" ref="mask">
            <div class="position-absolute text-center cell" style="margin-top: -125px">
              <img
                id="start-btn"
                ref="startBtn"
                src="../assets/start_btn.png"
                @click="start"
              />
            </div>
            <div>
              <img id="character" ref="character" src="../assets/niuniu.png" />
              <img id="character-ball" ref="characterBall" src="../assets/ball.png" />
            </div>
          </div>
        </div>
      </div>
      <div id="hint" ref="hint" class="max-600 text-center">
        <p
          style="
            height: 0px;
            margin-top: -100px;
            margin-bottom: 100px;
            color: #436cb6;
            font-size: 20px;
            font-weight: bold;
          "
        >
          Choose your ball
        </p>
      </div>
      <div class="max-600 text-center" style="height: 0px; margin: 0 auto">
        <img
          id="bowling-ball1"
          :ref="(el) => bowlingBalls.push(el)"
          class="bowling-ball bowling-ball1"
          src="../assets/ball.png"
          @click="chooseBall(0)"
        />
      </div>
      <div class="max-600 text-left" style="height: 0px; margin: 0 auto">
        <img
          id="bowling-ball2"
          :ref="(el) => bowlingBalls.push(el)"
          class="bowling-ball bowling-ball2"
          src="../assets/ball.png"
          @click="chooseBall(1)"
        />
      </div>
      <div class="max-600 text-right" style="height: 0px; margin: 0 auto">
        <img
          id="bowling-ball3"
          :ref="(el) => bowlingBalls.push(el)"
          class="bowling-ball bowling-ball3"
          src="../assets/ball.png"
          @click="chooseBall(2)"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.text-center {
  text-align: center;
}

.text-left {
  text-align: left;
}

.text-right {
  text-align: right;
}

.position-absolute {
  position: absolute;
}

.content {
  background-color: #fbb8ad;
  padding: 0rem;
}

.cell {
  margin: 0 auto;
  height: 100vw;
  width: 100vw;
  min-width: 300px;
  min-height: 300px;
  max-width: 600px;
  max-height: 600px;
}

.min-300 {
  min-width: 300px;
  min-height: 300px;
}

.max-600 {
  max-width: 600px;
  max-height: 600px;
}

.max-h-600 {
  height: 100vw;
  max-height: 600px;
}

.top-0 {
  top: 0;
}

.lane {
  background-color: #f4acb0;
  background-image: url("../assets/background.png");
  background-repeat: no-repeat;
  background-size: cover;
  border: 15px solid;
}

.bowling-ball {
  display: none;
  max-width: 50px;
  margin-top: -75px;
}

.bowling-ball1 {
  animation-name: bowl1;
  animation-duration: 3s;
  animation-iteration-count: 1;
  animation-direction: normal;
  animation-play-state: paused;
  animation-timing-function: ease;
}

@keyframes bowl1 {
  from {
    transform: translate(0) rotate(0) scale(1, 1);
  }

  to {
    transform: translate(0, -300px) rotate(180deg) scale(0.1, 0.1);
  }
}

.bowling-ball2 {
  animation-name: bowl2;
  animation-duration: 3s;
  animation-iteration-count: 1;
  animation-direction: normal;
  animation-play-state: paused;
  animation-timing-function: ease;
}

@keyframes bowl2 {
  from {
    transform: translate(0) rotate(0) scale(1, 1);
  }

  to {
    transform: translate(800%, -300px) rotate(180deg) scale(0.1, 0.1);
  }
}

.bowling-ball3 {
  animation-name: bowl3;
  animation-duration: 3s;
  animation-iteration-count: 1;
  animation-direction: normal;
  animation-play-state: paused;
  animation-timing-function: ease;
}

@keyframes bowl3 {
  from {
    transform: translate(0) rotate(0) scale(1, 1);
  }

  to {
    transform: translate(-800%, -300px) rotate(180deg) scale(0.1, 0.1);
  }
}

.pin_line1 {
  width: 22% !important;
  height: 22% !important;
}

.pin_line2 {
  width: 20% !important;
  height: 20% !important;
}

.pin_line3 {
  width: 18% !important;
  height: 18% !important;
}

.pin_line4 {
  width: 15% !important;
  height: 15% !important;
}

#start-btn {
  display: none;
  width: 30%;
  max-width: 150px;
  cursor: pointer;
  animation-name: start-btn;
  animation-duration: 1s;
  animation-iteration-count: infinite;
  animation-direction: normal;
  animation-timing-function: ease;
}
@keyframes start-btn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

#hint {
  margin: 0 auto;
  display: none;
  animation-name: start-btn;
  animation-duration: 1s;
  animation-iteration-count: infinite;
  animation-direction: normal;
  animation-timing-function: ease;
}

#character {
  width: 30%;
  max-width: 230px;
  margin-top: 70%;
  margin-left: -15%;
}

#character-ball {
  width: 10%;
  max-width: 50px;
  margin-top: 82%;
  margin-left: -27%;
}
</style>
