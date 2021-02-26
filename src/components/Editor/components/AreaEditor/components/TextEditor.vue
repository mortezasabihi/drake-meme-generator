<template>
  <div class="flex items-stretch">
    <div class="mr-4 w-full">
      <input
        v-model="text"
        ref="textInput"
        type="text"
        name="area_text"
        id="area_text"
        class="focus:ring-indigo-500 focus:border-indigo-500 block w-full h-full shadow-sm sm:text-sm border-gray-300 rounded-md"
      />
    </div>

    <div class="relative" v-click-outside="onClickOutside">
      <button
        class="w-12 rounded-md focus:outline-none p-3"
        :style="{ backgroundColor: color }"
        @click="visible = !visible"
      >
        <palette-icon />
      </button>

      <chrome-picker
        v-if="visible"
        :value="color"
        @input="color = $event.hex"
        class="absolute right-0 mt-3"
      />
    </div>
  </div>
</template>

<script>
import { Chrome } from "vue-color";
import PaletteIcon from "@/assets/palette.svg";

export default {
  name: "TextEditor",
  components: {
    "chrome-picker": Chrome,
    PaletteIcon
  },
  props: {
    value: {
      required: true,
      type: Object
    }
  },
  data() {
    return {
      text: "",
      visible: false,
      color: "#000"
    };
  },
  computed: {
    textValue() {
      return {
        text: this.text,
        color: this.color
      };
    }
  },
  watch: {
    value(val) {
      if (val.type !== "image") {
        const { color, text } = val.content;
        [this.color, this.text] = [color, text];

        if (!color && !text) {
          [this.color, this.text] = ["#000", ""];
        }
      } else {
        [this.color, this.text] = ["#000", ""];
      }
    },
    textValue(val) {
      this.$emit("change", val);
    }
  },
  created() {
    this.$nextTick(() => this.$refs.textInput.focus());
  },
  methods: {
    onClickOutside() {
      this.visible = false;
    }
  }
};
</script>
