<template>
  <div>
    <button @click="launchWheel()">LAUNCH</button
    ><button @click="resetWheel()">RESET</button><br />
    <p v-if="result">The result is : <span v-html="result.name"></span></p>
    <br /><br /><br /><br />
    <WheelOfFortune
      ref="wheel"
      :items="items"
      :first-item-index="firstItemIndex"
      :centered-indicator="true"
      indicator-position="top"
      :size="300"
      :result-variation="70"
      @wheel-start="wheelStartedCallback"
      @wheel-end="wheelEndedCallback"
      :counter-clockwise="true"
      :horizontal-content="false"
    />
    <ItemsManager
      class="item-manager"
      :initial-items="items"
      :initial-first-item-index="firstItemIndex"
      @update-items="resetWheel"
    />
  </div>
</template>

<script>
import WheelOfFortune from "./components/WheelOfFortune.vue";
import ItemsManager from "./components/ItemsManager.vue";
export default {
  name: "App",
  components: {
    WheelOfFortune,
    ItemsManager,
  },
  data() {
    return {
      firstItemIndex: { value: 0 },
      items: [
        { id: 1, name: "Banana" },
        { id: 2, name: "Apple" },
        { id: 3, name: "Orange<br>and Purple" },
        { id: 4, name: "Cherry" },
        { id: 5, name: "Strawberry" },
        { id: 6, name: "Grape" },
      ],
      result: null,
    };
  },
  watch: {
    firstItemIndex: {
      deep: true,
      handler() {
        this.$refs.wheel.reset();
      },
    },
  },
  methods: {
    wheelStartedCallback() {
      console.log("wheel started !");
    },
    wheelEndedCallback(value) {
      console.log("wheel ended !", value);
      this.result = value;
    },
    launchWheel() {
      this.$refs.wheel.launchWheel();
    },
    resetWheel(newItemList) {
      this.items = newItemList || this.items;
      this.$refs.wheel.reset();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.item-manager {
  margin-top: 50px;
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
}
</style>
