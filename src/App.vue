<template>
  <!-- <HelloWorld msg="Oracle Archive" /> -->

  <div class="d-flex centered">
    <p class="align-right bold">user #{{ currentUserId }}</p>

    <div class="box">
      <h3>{{ currentAnswer }}</h3>
    </div>
    <p class="bold">{{ counter + 1 }}/{{ flatData.length + 1 }}</p>
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import data from "./assets/oracleData.json";

export default {
  name: "App",
  components: {
    // HelloWorld,
  },
  data() {
    return {
      data: data,
      counter: 0,
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
        // and if counter is at last element
        if (this.counter == this.flatData.length) {
          //go to first element
          this.counter = 0;
        } else {
          //else count up
          this.counter++;
        }
        // if prev key is pressed
      } else if (e.keyCode === 37) {
        // and if counter is at first element
        if (this.counter == 0) {
          //go to last element
          this.counter = this.flatData.length;
          // else if the typewriter is not typing then go to next text
        } else {
          this.counter--;
        }
      }
    },
  },
  mounted() {
    window.addEventListener("keydown", this.handleKeyPress);
  },
  beforeUnmount() {
    window.removeEventListener("keydown", this.handleKeyPress);
  },
};
</script>

<style>
:root {
  --white: #ffffff;
  --black: #242424;
  --primaryColor: #ff4400;
  --primaryRGB: rgba(255,68,0,0.3)
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--primaryColor);

  margin-top: 60px;
}
.d-flex {
  display: flex;
  flex-direction: column;
}
.box {
  display: flex;
  align-items: center;
  justify-content: center;
  border: solid 1px var(--primaryColor);
  box-shadow: 7px 7px 1px 1px var(--primaryRGB);
  padding: 10px;
  height: 10rem;
  width: 60rem;
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
