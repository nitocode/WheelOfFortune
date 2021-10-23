<template>
  <div>
    <button @click="launchWheel()">LAUNCH</button
    ><button @click="resetWheel()">RESET</button><br />
    <p v-if="result">The result is : <span v-html="result.name"></span></p>
    <br /><br /><br /><br />
    <WheelOfFortune
      v-if="wheelActive"
      ref="wheel"
      :items="items"
      :first-item-index="firstItemIndex"
      :centered-indicator="wheelSettings.centeredIndicator"
      :indicator-position="wheelSettings.indicatorPosition"
      :size="wheelSettings.size"
      :result-variation="wheelSettings.resultVariation"
      @wheel-start="wheelStartedCallback"
      @wheel-end="wheelEndedCallback"
      :counter-clockwise="true"
      :horizontal-content="false"
    />
    <div class="manager">
      <ItemsManager
        class="item-manager"
        :initial-items="items"
        :initial-first-item-index="firstItemIndex"
        @update-items="resetWheel"
      />
      <WheelManager
        :initial-settings="wheelSettings"
        @hard-reset="onHardReset"
      />
    </div>
  </div>
</template>

<script>
import WheelOfFortune from "./components/WheelOfFortune.vue";
import ItemsManager from "./components/ItemsManager.vue";
import WheelManager from "./components/WheelManager.vue";
export default {
  name: "App",
  components: {
    WheelOfFortune,
    ItemsManager,
    WheelManager,
  },
  data() {
    return {
      wheelActive: true,
      firstItemIndex: { value: 0 },
      wheelSettings: {
        centeredIndicator: true,
        indicatorPosition: "top",
        size: 300,
        resultVariation: 70,
      },
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
    onHardReset() {
      this.wheelActive = false;
      setTimeout(() => {
        this.wheelActive = true;
      }, 20);
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.manager {
  margin-top: 50px;
  display: flex;
  flex-direction: row;
  align-items: top;
  justify-content: space-around;
  .item-manager {
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
  }
}
</style>
