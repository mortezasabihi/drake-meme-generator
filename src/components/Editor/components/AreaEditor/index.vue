<template>
  <div class="bg-white w-full p-6 rounded-tr-3xl rounded-bl-3xl">
    <h2 class="text-5xl font-bold pb-4 mb-5 border-b-2 border-gray-600">
      # Area {{ area }}
    </h2>

    <type-selector :value="type" @input="handleTypeChange" />

    <text-editor
      v-if="type === 'text'"
      :value="area === 1 ? areaOne : areaTwo"
      @change="handleTextChange"
    />
    <image-editor
      v-else
      :value="area === 1 ? areaOne : areaTwo"
      @change="handleImageChange"
    />
  </div>
</template>

<script>
import TypeSelector from "./components/TypeSelector";
import TextEditor from "./components/TextEditor";
import ImageEditor from "./components/ImageEditor";

export default {
  name: "AreaEditor",
  components: {
    TypeSelector,
    TextEditor,
    ImageEditor
  },
  props: {
    area: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      type: "text",
      areaOne: {
        type: "",
        content: ""
      },
      areaTwo: {
        type: "",
        content: ""
      }
    };
  },
  watch: {
    area(val) {
      if (val === 1) {
        this.type = this.areaOne.type || "text";
      } else if (val === 2) {
        this.type = this.areaTwo.type || "text";
      }
    }
  },
  methods: {
    handleTypeChange(type) {
      this.type = type;

      if (this.area === 1) {
        this.areaOne = {
          type,
          content: this.areaOne.content || ""
        };

        if (this.type === "image") {
          this.areaOne.content =
            typeof this.areaOne.content === "object"
              ? ""
              : this.areaOne.content;
        }
      } else if (this.area === 2) {
        this.areaTwo = {
          type,
          content: this.areaTwo.content || ""
        };

        if (this.type === "image") {
          this.areaTwo.content =
            typeof this.areaTwo.content === "object"
              ? ""
              : this.areaTwo.content;
        }
      }
    },
    handleImageChange(e) {
      const obj = {
        type: "image",
        content: e
      };

      if (this.area === 1) {
        this.areaOne = obj;
      } else if (this.area === 2) {
        this.areaTwo = obj;
      }
    },
    handleTextChange(e) {
      const obj = {
        type: "text",
        content: e
      };

      if (this.area === 1) {
        this.areaOne = obj;
      } else if (this.area === 2) {
        this.areaTwo = obj;
      }
    }
  }
};
</script>
