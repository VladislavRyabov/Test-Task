<template>
  <div class="popup-wrapper">
    <div class="v-popup">
      <div class="v-popup__header">
        <button @click="closePopup" class="v-popup__close">Close</button>
      </div>
      <div class="v-popup__content">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "vPopup",
  props: {},
  datd() {
    return {};
  },
  methods: {
    closePopup() {
      this.$emit("closePopup");
    },
  },
};
</script>

<style lang="scss">
.popup-wrapper {
  background-color: #353535c2;
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  right: 0;
  left: 0;
  top: 0;
  bottom: 0;
}
.v-popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 500px;
  height: 500px;
  background: rgb(239 239 239);
  border: none;
  border-radius: 5px;

  @media screen and (max-width: 550px) {
    width: 300px;
    height: 500px;
  }
  &__header {
    display: flex;
  }
  &__close {
    border: none;
    margin-left: auto;
    padding: 20px;
    cursor: pointer;
  }
}
</style>
