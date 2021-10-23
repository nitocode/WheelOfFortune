<template>
  <div>
    <strong>Wheel manager</strong>
    <!-- CENTERED INDICATOR -->
    <div>
      <label for="centered">Centered indicator</label>
      <input
        id="centered"
        type="checkbox"
        v-model="settings.centeredIndicator"
      />
    </div>
    <!-- INDICATOR POSITION -->
    <div>
      <label for="position">Indicator position</label>
      <select
        name="position"
        id="position"
        v-model="settings.indicatorPosition"
      >
        <option value="top">Top</option>
        <option value="right">Right</option>
        <option value="bottom">Bottom</option>
        <option value="left">Left</option>
      </select>
    </div>
    <!-- SIZE -->
    <div>
      <label for="size">Size</label>
      <input type="number" v-model="settings.size" />
    </div>
    <!-- VARIATION -->
    <div>
      <label for="variation">Variation</label>
      <input type="number" v-model="settings.resultVariation" />
    </div>
    <!-- APPLY HARD RESET -->
    <div>
      <button @click="hardReset">Hard reset</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "WheelManager",
  emits: ["hardReset"],
  props: {
    initialSettings: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      settings: {},
    };
  },
  mounted() {
    this.settings = this.initialSettings;
  },
  methods: {
    hardReset() {
      this.$emit("hard-reset");
    },
  },
};
</script>

<style lang="scss">
</style>