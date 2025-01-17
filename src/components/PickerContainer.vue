<template>
  <div
    ref="container"
    class="modal-container"
    :class="{ modal: modal, show: showModal }"
  >
    <div class="dialog-box-container" @click.stop @click.prevent.stop>
      <slot name="header">
        <main-header
          :title="title"
          :showClose="modal"
          @close="$emit('close')"
        />
      </slot>
      <div class="selects-container">
        <slot />
      </div>
      <slot name="submit">
        <button
          class="submit-button"
          :style="{ background: color }"
          @click="$emit('submit')"
        >
          {{ submitTitle }}
        </button>
      </slot>
    </div>
    <div class="container-mask" @click="$emit('close')" />
  </div>
</template>

<script lang="ts">
import Vue from "vue";

// components
import MainHeader from "./MainHeader.vue";

export default Vue.extend({
  name: "PickerContainer",
  components: { MainHeader },
  props: {
    title: {
      type: String,
      default: undefined,
    },
    submitTitle: {
      type: String,
      default: undefined,
    },
    color: {
      type: String,
      default: "#188EF2",
    },
    modal: {
      type: Boolean,
      default: false,
    },
    showModal: {
      type: Boolean,
      default: false,
    },
  },
  mounted() {
    if (this.modal) {
      (this.$refs.container as HTMLDivElement).addEventListener(
        "wheel",
        this.onWheel
      );
    }
  },
  beforeDestroy() {
    if (this.modal) {
      (this.$refs.container as HTMLDivElement).removeEventListener(
        "wheel",
        this.onWheel
      );
    }
  },
  methods: {
    onWheel(event: Event) {
      event.preventDefault();
    },
  },
});
</script>

<style scoped lang="scss">
.modal-container {
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
  transition: opacity 0.3s;
  .container-mask {
    display: none;
    z-index: 50;
    background-color: rgba($color: #43464f, $alpha: 0.7);
  }

  &.modal {
    position: fixed;
    z-index: 99998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    visibility: hidden;
    opacity: 0;

    &.show {
      visibility: visible;
      opacity: 1;

      .container-mask {
        position: absolute;
        display: block;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
      }
    }
  }
}

.dialog-box-container {
  display: flex;
  flex-direction: column;
  height: 328px;
  min-width: 328px;
  background: #ffffff;
  border-radius: 10px;
  padding: 16px;
  z-index: 100;
}

.selects-container {
  flex-grow: 1;
  display: flex;

  .select:not(:first-child) {
    margin-left: 6px;
  }
}

html[dir="rtl"] .selects-container {
  .select:not(:first-child) {
    margin-left: 0;
    margin-right: 6px;
  }
}

.submit-button {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin: 10px 0;
  color: white;
  box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.15);
  border-radius: 16px;
  font-weight: bold;
  font-size: 14px;
  height: 48px;
  text-align: center;
  cursor: pointer;
  width: 100%;
  border: 0;
  outline: none;

  &:hover {
    opacity: 0.9;
  }
  &:active {
    border: 0;
    opacity: 0.8;
  }
}
</style>
