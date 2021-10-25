<template>
  <div>
    <button v-if="!result" @click="launchWheel()">LAUNCH</button
    ><button v-if="result" @click="onHardReset()">HARD RESET</button
    ><button v-if="result" @click="onSoftReset()">SOFT RESET</button><br />
    <br /><br />
    <WheelOfFortune
      v-if="wheelActive"
      ref="wheel"
      @click="launchWheel"
      :items="items"
      :first-item-index="firstItemIndex"
      :centered-indicator="wheelSettings.centeredIndicator"
      :indicator-position="wheelSettings.indicatorPosition"
      :size="wheelSettings.size"
      :display-Shadow="wheelSettings.displayShadow"
      :display-border="wheelSettings.displayBorder"
      :display-indicator="wheelSettings.displayIndicator"
      :duration="wheelSettings.duration"
      :result-variation="wheelSettings.resultVariation"
      :easing="wheelSettings.easing"
      @wheel-start="wheelStartedCallback"
      @wheel-end="wheelEndedCallback"
      :counter-clockwise="wheelSettings.counterClockwise"
      :horizontal-content="wheelSettings.horizontalContent"
      :base-display="wheelSettings.baseDisplay"
      :base-size="wheelSettings.baseSize"
      :base-display-indicator="wheelSettings.baseDisplayIndicator"
      :base-display-shadow="wheelSettings.baseDisplayShadow"
      :base-background="wheelSettings.baseBackground"
    >
      <template #baseContent>
        <div
          v-if="wheelSettings.baseHtmlContent"
          v-html="wheelSettings.baseHtmlContent"
        ></div>
      </template>
    </WheelOfFortune>

    <p v-if="result">The result is : <span v-html="result.name"></span></p>

    <div class="manager">
      <ItemsManager
        class="item-manager"
        :initial-items="items"
        :initial-first-item-index="firstItemIndex"
        @update-items="onSoftReset"
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
        displayShadow: true,
        duration: 5,
        resultVariation: 70,
        easing: "bounce",
        counterClockwise: true,
        horizontalContent: false,
        displayBorder: true,
        displayIndicator: true,
        baseDisplay: true,
        baseSize: 100,
        baseDisplayShadow: true,
        baseDisplayIndicator: true,
        baseBackground: "#EEAA33",
        baseHtmlContent: "Awesome<br>Wheel",
      },
      items: [
        { id: 1, name: "Banana", htmlContent: "Banana", background: "" },
        { id: 2, name: "Apple", htmlContent: "Apple", background: "" },
        {
          id: 3,
          name: "Orange and Purple",
          htmlContent: "Orange<br>and Purple",
          background: "",
        },
        { id: 4, name: "Cherry", htmlContent: "Cherry", background: "" },
        {
          id: 5,
          name: "Strawberry",
          htmlContent: "Strawberry",
          background: "",
        },
        { id: 6, name: "Grape", htmlContent: "Grape", background: "" },
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
    onSoftReset(newItemList) {
      this.items = newItemList || this.items;
      this.$refs.wheel.reset();
    },
    onHardReset() {
      this.wheelActive = false;
      this.result = null;
      setTimeout(() => {
        this.wheelActive = true;
      }, 20);
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Ubuntu, Helvetica, Arial, sans-serif;
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
