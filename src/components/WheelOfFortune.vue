<template>
  <div class="wheel-container" :class="`indicator-${indicatorPosition}`">
    <div
      @click="launchWheel()"
      class="wheel"
      :class="`easing-${easing}`"
      :style="{
        width: `${size}px`,
        height: `${size}px`,
        transitionDuration: `${duration}s`,
      }"
    >
      <div
        v-for="(item, index) in items"
        :key="item.id"
        class="wheel-item"
        :style="{
          transform: `rotate(${itemAngle * index}deg) skewY(${-(
            90 - itemAngle
          )}deg)`,
        }"
      >
        <div
          class="content"
          :class="{ 'horizontal-content': horizontalContent }"
          :style="{
            transform: `skewY(${90 - itemAngle}deg) rotate(${
              itemAngle / 2
            }deg)`,
          }"
        >
          <span v-html="item.name"></span>
        </div>
      </div>
    </div>
    <!-- <p v-if="itemSelected">The result will be : {{ itemSelected.name }}</p> -->
  </div>
</template>

<script>
export default {
  name: "WheelOfFortune",
  emits: ["wheelStart", "wheelEnd"],
  props: {
    items: {
      type: Object,
      required: true,
      validator(value) {
        return value.length >= 4;
      },
    },
    firstItemIndex: {
      type: Object,
      required: false,
      default() {
        return { value: 0 };
      },
    },
    centeredIndicator: {
      type: Boolean,
      required: false,
      default: false,
    },
    indicatorPosition: {
      type: String,
      required: false,
      default: "top",
      validator(value) {
        return ["top", "right", "bottom", "left"].includes(value);
      },
    },
    size: {
      type: Number,
      required: false,
      default: 300,
    },
    duration: {
      type: Number,
      required: false,
      default: 4,
    },
    resultVariation: {
      type: Number,
      required: false,
      default: 0,
      validator(value) {
        return value >= 0 && value <= 100;
      },
    },
    easing: {
      type: String,
      required: false,
      default: "ease",
      validator(value) {
        return ["ease", "bounce"].includes(value);
      },
    },
    counterClockwise: {
      type: Boolean,
      required: false,
      default: false,
    },
    horizontalContent: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  data() {
    return {
      itemSelected: null,
      processingLock: false,
    };
  },
  computed: {
    itemAngle: function () {
      return 360 / this.items.length;
    },
    startingAngle: function () {
      if (this.centeredIndicator) {
        return (
          -1 * this.firstItemIndex.value * this.itemAngle - this.itemAngle / 2
        );
      } else {
        return -1 * this.firstItemIndex.value * this.itemAngle;
      }
    },
    degreesVariation: function () {
      if (!this.resultVariation) {
        return 0;
      }
      const minDegreesVariation =
        (((this.itemAngle / 2) * this.resultVariation) / 100) * -1;
      const maxDegreesVariation =
        ((this.itemAngle / 2) * this.resultVariation) / 100;
      // Return random value between min and max degrees variation
      return Number(
        (
          Math.random() * (maxDegreesVariation - minDegreesVariation) +
          minDegreesVariation
        ).toFixed(2)
      );
    },
    counterClockWiseOperator: function () {
      return this.counterClockwise ? -1 : 1;
    },
  },
  mounted() {
    this.reset();
    document.querySelector(".wheel").addEventListener("transitionend", () => {
      this.processingLock = false;
      this.$emit("wheel-end", this.itemSelected);
    });
  },
  methods: {
    reset() {
      this.itemSelected = null;
      document.querySelector(
        ".wheel"
      ).style.transform = `rotate(${this.startingAngle}deg)`;
    },
    launchWheel() {
      if (this.processingLock) {
        return;
      }
      this.processingLock = true;
      const wheelResult = Math.floor(Math.random() * this.items.length + 1);
      const wheelElt = document.querySelector(".wheel");

      console.log("wheelResult", wheelResult);
      this.itemSelected = this.items[wheelResult - 1];

      wheelElt.style.transform = `rotate(${
        this.counterClockWiseOperator * (360 * 3) +
        -(wheelResult - 1) * this.itemAngle -
        this.itemAngle / 2 +
        this.degreesVariation
      }deg)`;
      this.$emit("wheel-start");
    },
  },
};
</script>

<style lang="scss">
.wheel-container {
  position: relative;
  transition: transform 1s ease-in-out;
  &.indicator-top {
    transform: rotate(0deg);
  }
  &.indicator-right {
    transform: rotate(90deg);
  }
  &.indicator-bottom {
    transform: rotate(180deg);
  }
  &.indicator-left {
    transform: rotate(270deg);
  }
}
.wheel {
  background: green;
  border-radius: 50%;
  margin: auto;
  overflow: hidden;
  transition: transform cubic-bezier(0.34, 1.56, 0.64, 1);

  &.easing-ease {
    transition: transform ease-in-out;
  }
  &.easing-bounce {
    transition: transform cubic-bezier(0.34, 1.56, 0.64, 1);
  }

  &-item {
    overflow: hidden;
    position: absolute;
    top: 0;
    right: 0;
    width: 50%;
    height: 50%;
    transform-origin: 0% 100%;
  }
  &-item:nth-child(odd) {
    background-color: skyblue;
  }
  &-item:nth-child(even) {
    background-color: pink;
  }

  .content {
    position: absolute;
    left: -100%;
    width: 200%;
    height: 200%;
    text-align: center;
    transform: skewY(30deg) rotate(0deg);
    padding-top: 20px;

    &.horizontal-content {
      left: initial;
      right: 100%;
      width: 60%;
      height: 250%;
      text-align: right;
      span {
        display: block;
        transform: rotate(270deg);
      }
    }
  }
}
</style>