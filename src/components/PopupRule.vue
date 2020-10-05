<template>
  <div class="wrapper-popup" v-bind:class="Openpopup">
    <div class="rule">
      <h3>Luật chơi</h3>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Accusamus
        minima tenetur iure sed ducimus, quis necessitatibus, nesciunt harum id
        beatae excepturi enim! Nemo maiores nisi sapiente debitis reiciendis.
        Magnam, vel.
      </p>
      <button v-on:click="handleConfirm" class="confirm">Đã hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup-rule",
  props: {
    isOpenpopup: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {};
  },
  methods: {
    handleConfirm() {
      this.$emit("handleConfirm");
    },
  },
  computed: {
    Openpopup() {
      return {
        "open-popup": this.isOpenpopup,
      };
    },
  },
};
</script>

<style>
.wrapper-popup {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.4);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
}

.open-popup .rule {
  transform: translate(-50%, -50%) scale(1);
}

.rule {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%) scale(1.2);

  background-color: seashell;
  font-family: Arial, Helvetica, sans-serif;
  width: 350px;
  padding: 20px;

  transition: all 0.3s ease;
}
.rule h3 {
  margin-bottom: 10px;
}

.rule .confirm {
  cursor: pointer;
  margin-top: 20px;
  padding: 8px 15px;
  /* background-color: rgb(134, 129, 129); */
  transition: all 0.3s ease;
}
.rule .confirm:hover {
  background-color: teal;
  color: #fff;
}
</style>