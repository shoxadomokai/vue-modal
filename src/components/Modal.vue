<template>
  <div>
    <div
      v-if="value && hideBackdrop"
      class="modal-backdrop"
      @click.prevent="close"
    ></div>
    <transition :name="animation">
      <div
        :ref="`modal-${name}`"
        :id="`modal-${name}`"
        class="modal"
        v-show="value"
        :class="{ rounded: rounded }"
        tabindex="-1"
        :aria-labelledby="`modal-${name}-title`"
      >
        <button
          v-if="!hideClose"
          @click.prevent="close"
          class="close"
          aria-label="Close"
        >
          <img src="@/assets/close.png" alt="" />
        </button>
        <h4 class="modal__title" v-if="!hideTitle" :id="`modal-${name}-title`">
          <slot name="header">{{ title }}</slot>
        </h4>
        <div class="modal__body">
          <slot></slot>
        </div>
        <div v-if="!hideFooter" class="modal__footer">
          <slot name="footer">{{ footer }}</slot>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "Modal",
  watch: {
    size(newValue, oldValue) {
      this.modal.classList.remove(`modal-${oldValue}`);
      this.setSize();
    },
    text(newValue, oldValue) {
      this.modal.classList.remove(`text-${oldValue}`);
      this.setTextAlignment();
    },
    bg(newValue, oldValue) {
      if (this.supportedColours.includes(oldValue)) {
        this.modal.classList.remove(`${oldValue}-bg`);
      }
      this.setBackground();
    },
    rounded() {
      this.setBorderRadius();
    },
    value(value) {
      if (value) {
        document.addEventListener("keydown", this.close);
      } else {
        document.removeEventListener("keydown", this.close, true);
      }
    }
  },
  props: {
    value: {
      required: true
    },
    name: {
      required: true
    },
    rounded: {
      type: [String, Number, Boolean],
      default() {
        return false;
      }
    },
    bg: {
      type: String,
      default() {
        return "white";
      }
    },
    size: {
      default() {
        return "md";
      }
    },
    text: {
      default() {
        return "center";
      }
    },
    hideTitle: {
      default() {
        return false;
      }
    },
    hideClose: {
      default() {
        return false;
      }
    },
    hideFooter: {
      default() {
        return false;
      }
    },
    hideBackdrop: {
      default() {
        return false;
      }
    },
    footer: {
      default() {
        return "This is a footer";
      }
    },
    title: {
      default() {
        return "This is a title";
      }
    },
    animation: {
      default() {
        return "flip";
      }
    }
  },
  computed: {
    modal() {
      return document.getElementById(`modal-${this.name}`);
    },
    modalFooter() {
      return this.modal.querySelector(".modal__footer");
    },
    supportedColours() {
      return [
        "white",
        "red",
        "orange",
        "yellow",
        "green",
        "blue",
        "indigo",
        "violet"
      ];
    }
  },
  methods: {
    close(event) {
      if (!event) this.$emit("close", false);
      if (event && event.keyCode === 27) {
        this.$emit("close", false);
      }
    },
    setBorderRadius() {
      let rounded = this.rounded;
      if (typeof rounded === "number" || !rounded.match("[a-zA-Z]")) {
        rounded += "px";
      }
      this.modal.style.borderRadius = rounded;
      this.modalFooter.style.borderRadius = `0 0 ${rounded} ${rounded}`;
    },
    setBackground() {
      if (this.supportedColours.includes(this.bg)) {
        this.modal.classList.add(`${this.bg}-bg`);
      } else {
        this.modal.style.backgroundColor = this.bg;
      }
    },
    setSize() {
      this.modal.classList.add(`modal-${this.size}`);
    },
    setTextAlignment() {
      this.modal.classList.add(`text-${this.text}`);
    }
  },
  mounted() {
    this.setBorderRadius();
    this.setBackground();
    this.setSize();
    this.setTextAlignment();
  }
};
</script>

<style lang="scss" scoped>
.modal-backdrop {
  background: rgba(19, 50, 82, 0.1);
  backdrop-filter: blur(5px);
  position: fixed;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  z-index: 9998;
}
// root
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  max-height: 80%;
  transform: translate(-50%, -50%);
  box-shadow: 0px 5px 20px rgba(19, 50, 82, 0.2);
  // box-shadow: 0px 3px 7px rgba(19, 50, 82, 0.1);
  z-index: 9999;

  > * {
    box-sizing: border-box !important;
  }

  .modal__title {
    font-size: Clamp(1.5rem, 1.5vw, 2rem);
    padding: 20px;
    text-transform: capitalize;
    margin-bottom: 0;
  }

  .modal__body {
    padding: 10px 20px 50px 20px;
    width: 100%;

    img {
      max-width: 100%;
    }
  }

  .modal__footer {
    background: wheat;
    padding: 20px;

    * {
      margin: 0;
    }
  }

  .close {
    all: unset;
    cursor: pointer;
    position: absolute;
    background: wheat;
    padding: 10px;
    width: 40px;
    height: 40px;
    top: -20px;
    right: 5px;
    border-radius: 50%;
    box-shadow: 0px 3px 7px rgba(19, 50, 82, 0.1);
    display: grid;
    place-items: center;

    img {
      max-height: 70%;
      max-width: 70%;
    }
  }
}
// colours
.modal {
  &.rounded {
    border-radius: 10px;
  }

  &.white-bg {
    background: white;
  }
  &.red-bg {
    background: orangered;
  }
  &.orange-bg {
    background: orange;
  }
  &.yellow-bg {
    background: yellow;
  }
  &.green-bg {
    background: green;
  }
  &.blue-bg {
    background: dodgerblue;
  }
  &.indigo-bg {
    background: indigo;
  }
  &.violet-bg {
    background: violet;
  }
}

// size
.modal {
  transition: width 0.3s ease;
  &.modal-sm {
    width: Min(80vw, 400px);
  }
  &.modal-md {
    width: Min(80vw, 600px);
  }
  &.modal-lg {
    width: Min(80vw, 800px);
  }
}

//text
.modal {
  &.text-center {
    text-align: center !important;
  }
  &.text-left {
    text-align: left !important;
  }
  &.text-right {
    text-align: right !important;
  }
}

// animations scale
.scale-enter-active {
  animation: scale 0.5s cubic-bezier(0.39, 0.575, 0.565, 1);
}

.scale-leave-active {
  animation: scale 0.5s cubic-bezier(0.39, 0.575, 0.565, 1) reverse;
}
@keyframes scale {
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0);
  }
  100% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }
}

// animations swirl
.swirl-enter-active {
  animation: swirl 0.6s ease-out;
}

.swirl-leave-active {
  animation: swirl 0.6s ease-out reverse;
}
@keyframes swirl {
  0% {
    transform: translate(-50%, -50%) rotate(-540deg) scale(0);
    opacity: 0;
  }
  100% {
    transform: translate(-50%, -50%) rotate(0) scale(1);
    opacity: 1;
  }
}

// animations flip
.flip-enter-active {
  animation: flip 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.flip-leave-active {
  animation: flip 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94) reverse;
}

@keyframes flip {
  0% {
    transform: translate(-50%, -50%) rotate3d(-1, 1, 0, -80deg);
    opacity: 0;
  }
  100% {
    transform: translate(-50%, -50%) rotate3d(1, 1, 0, 0deg);
    opacity: 1;
  }
}

// animations swing
.swing-enter-active {
  animation: swing 1s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.swing-leave-active {
  animation: swing 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) reverse;
}

@keyframes swing {
  0% {
    transform: translate(-50%, -50%) rotateX(-100deg);
    transform-origin: top;
    opacity: 0;
  }
  100% {
    transform: translate(-50%, -50%) rotateX(0deg);
    transform-origin: top;
    opacity: 1;
  }
}
// animations swing
.slide-enter-active {
  animation: slide 0.6s cubic-bezier(0.23, 1, 0.32, 1);
}

.slide-leave-active {
  animation: slide 0.6s cubic-bezier(0.23, 1, 0.32, 1) reverse;
}

@keyframes slide {
  0% {
    transform: translate(-50%, -1000px) scaleY(2.5) scaleX(0.2);
    transform-origin: 50% 0%;
    filter: blur(40px);
    opacity: 0;
  }
  100% {
    transform: translate(-50%, -50%) scaleY(1) scaleX(1);
    transform-origin: 50% 50%;
    filter: blur(0);
    opacity: 1;
  }
}
</style>
