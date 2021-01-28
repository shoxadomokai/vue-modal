<template>
  <main class="container">
    <div class="options">
      <div class="block">
        <label for="animations">Animations</label>
        <select v-model="animation" class="option" id="animations">
          <option v-for="anim in animations" :key="anim">{{ anim }}</option>
        </select>
      </div>
      <div class="block">
        <label for="alignments">Text Alignment</label>
        <select v-model="alignment" class="option" id="alignments">
          <option v-for="align in alignments" :key="align">{{ align }}</option>
        </select>
      </div>
      <div class="block">
        <label for="sizes">Modal Size</label>
        <select v-model="size" class="option" id="sizes">
          <option v-for="s in sizes" :key="s">{{ s }}</option>
        </select>
      </div>

      <div class="block">
        <label for="background">Background Colour</label>
        <input
          v-model="background"
          type="color"
          class="option"
          list="background"
          id="background"
        />
        <datalist id="background">
          <option v-for="back in backgrounds" :key="back">{{ back }}</option>
        </datalist>
      </div>
      <div class="option checkbox">
        <label for="hideTitle">Hide Title</label>
        <input type="checkbox" v-model="hideTitle" name="" id="hideTitle" />
      </div>
      <div class="option checkbox">
        <label for="hideFooter">Hide Footer</label>
        <input type="checkbox" v-model="hideFooter" name="" id="hideFooter" />
      </div>
      <div class="option checkbox">
        <label for="hideClose">Hide Close Button</label>
        <input type="checkbox" v-model="hideClose" name="" id="hideClose" />
      </div>
      <div class="option checkbox">
        <label for="hideBackdrop">Show Backdrop</label>
        <input
          type="checkbox"
          v-model="hideBackdrop"
          name=""
          id="hideBackdrop"
        />
      </div>
      <div class="range block">
        <label for="radius">Border Radius ({{ radius }}px)</label>
        <input v-model="radius" type="range" />
      </div>
    </div>
    <div class="button-wrapper">
      <button class="btn" @click="openModal">
        {{ !modalOpen ? "Open" : "Close" }} Modal
      </button>
    </div>
    <modal-direction
      :name="'main'"
      :value="modalOpen"
      :rounded="radius"
      :size="size"
      :text="alignment"
      :footer="'This is another way to pass footers or headers'"
      :animation="animation"
      :bg="background"
      :hideTitle="hideTitle"
      :hideFooter="hideFooter"
      :hideClose="hideClose"
      :hideBackdrop="hideBackdrop"
      @close="modalOpen = false"
    >
      <template v-slot:header>
        Here might be a page title
      </template>
      <p>This is the body of the modal</p>
    </modal-direction>
  </main>
</template>

<script>
import ModalDirection from "@/components/Modal";
export default {
  components: {
    ModalDirection
  },
  data() {
    return {
      modalOpen: false,
      animation: "flip",
      alignment: "center",
      size: "md",
      background: "#ffffff",
      radius: "10",
      hideTitle: false,
      hideClose: false,
      hideBackdrop: false,
      hideFooter: false
    };
  },
  computed: {
    animations() {
      return ["scale", "swirl", "flip", "swing", "slide"];
    },
    alignments() {
      return ["left", "center", "right"];
    },
    sizes() {
      return ["sm", "md", "lg"];
    },
    backgrounds() {
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
    openModal() {
      this.modalOpen = !this.modalOpen;
    }
  }
};
</script>

<style lang="scss">
.container {
  padding: Max(10vh, 100px);
  max-width: 1024px;
  margin: auto;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
}
.options {
  display: flex;
  align-items: flex-end;
  flex-flow: row wrap;
  gap: 5px;
  width: 100%;
  justify-content: center;

  .block {
    label {
      display: block;
    }
  }

  .option {
    min-width: 180px;
    text-transform: capitalize;
    padding: 10px;
    height: 40px;
    border-radius: 5px;
    background: rgba(black, 0.05);
  }

  .option.checkbox {
    align-items: center;
    display: flex;
    justify-content: space-between;
    border: 0.5px solid black;

    label {
      font-weight: bold;
    }
  }
  .range {
    min-width: 180px;

    input {
      width: 100%;
    }
  }
}

.button-wrapper {
  text-align: center;
  margin-top: 30px;
  position: absolute;
  bottom: 50px;
  left: 50%;
  transform: translateX(-50%);

  .btn {
    all: unset;
    cursor: pointer;
    padding: 10px 30px;
    background: dodgerblue;
    border-radius: 5px;
    color: white;
    transition: 0.3s all ease;
    text-align: center;

    &:hover {
      background: blue;
    }
  }
}
</style>
