<template>
  <div class="d-flex-col centered">
    <p class="align-right bold">user #{{ currentUserId }}</p>

    <div class="d-flex-center border-box">
      <h3>{{ currentAnswer }}</h3>
    </div>
    <div class="d-flex-row footer bold">
      <button @click="prev">&#60;</button>
      <p>{{ counter + 1 }}/{{ flatData.length }}</p>
      <button @click="next">&#62;</button>
    </div>
  </div>
</template>

<script>
import data from "./assets/oracleData.json";

export default {
  name: "App",
  data() {
    return {
      data: data,
      counter: 0,
      touchstartX: 0,
      touchstartY: 0,
      touchendX: 0,
      touchendY: 0,
      
    };
  },
  computed: {
    flatData() {
      const flattenedData = [];
      this.data.forEach((item, index) => {
        item.forEach((answer) => {
          flattenedData.push({ userId: index + 1, answer: answer });
        });
      });
      return flattenedData;
    },
    currentAnswer() {
      return this.flatData[this.counter].answer;
    },
    currentUserId() {
      return this.flatData[this.counter].userId;
    },
  },
  methods: {
    handleKeyPress(e) {
      //if next key is pressed
      if (e.keyCode === 39) {
        this.next();
        // if prev key is pressed
      } else if (e.keyCode === 37) {
        this.prev();
      }
    },

    next() {
      // counter is at last element
      if (this.counter == this.flatData.length-1) {
        //go to first element
        this.counter = 0;
      } else {
        //else count up
        this.counter++;
      }
    },
    prev() {
      // if counter is at first element
      if (this.counter == 0) {
        //go to last element
        this.counter = this.flatData.length-1;
        // else if the typewriter is not typing then go to next text
      } else {
        this.counter--;
      }
    },
    handleTouchGesture() {
      if (this.touchendX < this.touchstartX) {
        this.next();
        console.log("Swiped Left");
      }

      if (this.touchendX > this.touchstartX) {
        console.log("Swiped Right");
        this.prev();
      }
    },
  },
  mounted() {
    window.addEventListener("keydown", this.handleKeyPress);
    window.addEventListener(
      "touchstart",
      function (event) {
        this.touchstartX = event.changedTouches[0].screenX;
        this.touchstartY = event.changedTouches[0].screenY;
      },
      false
    );

    window.addEventListener(
      "touchend",
      function (event) {
        this.touchendX = event.changedTouches[0].screenX;
        this.touchendY = event.changedTouches[0].screenY;
        this.handleTouchGesture();
      },
      false
    );
  },
  beforeUnmount() {
    window.removeEventListener("keydown", this.handleKeyPress);
    //TODO: remove touchgesture
  },
};
</script>

<style>
:root {
  --white: #ffffff;
  --black: #242424;
  --primaryColor: #ff4400;
  --primaryRGB: rgba(255, 68, 0, 0.3);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--primaryColor);

  margin-top: 60px;
}
.d-flex-col {
  display: flex;
  flex-direction: column;
}

.footer {
  display: flex;
  justify-content: space-between;
}
button {
  all: unset;
  cursor: pointer;
  font-size: 40px;
}
.d-flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}
.border-box {
  border: solid 1px var(--primaryColor);
  box-shadow: 7px 7px 1px 1px var(--primaryRGB);
  padding: 10px;
  height: 20rem;
  width: 60rem;
  max-width: 80vw;
}
.centered {
  position: fixed;
  top: 50%;
  left: 50%;
  /* bring your own prefixes */
  transform: translate(-50%, -50%);
}
.align-right {
  align-self: flex-start;
}
.bold {
  font-weight: bold;
}
</style>
