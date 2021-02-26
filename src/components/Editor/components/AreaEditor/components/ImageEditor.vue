<template>
  <div
    class="flex justify-center px-6 pt-5 pb-6 border-2 border-gray-300 border-dashed rounded-md relative"
    @dragover.prevent
    @drop.prevent="onDrop"
  >
    <div v-if="!value.content" class="space-y-1 text-center">
      <svg
        class="mx-auto h-12 w-12 text-gray-400"
        stroke="currentColor"
        fill="none"
        viewBox="0 0 48 48"
        aria-hidden="true"
      >
        <path
          d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
      <div class="flex text-sm text-gray-600">
        <label
          for="file-upload"
          class="relative cursor-pointer bg-white rounded-md font-medium text-indigo-600 hover:text-indigo-500 focus-within:outline-none focus-within:ring-2 focus-within:ring-offset-2 focus-within:ring-indigo-500"
        >
          <span>Select a file</span>
          <input
            id="file-upload"
            name="file-upload"
            type="file"
            class="sr-only"
            accept="image/png, image/jpeg, image/svg+xml"
            @change="onChange"
          />
        </label>
        <p class="pl-1">or drag and drop</p>
      </div>
      <p class="text-xs text-gray-500">
        PNG, JPG, SVG up to {{ maxFileSize }}MB
      </p>
    </div>

    <template v-else>
      <button
        class="focus:outline-none absolute top-0 right-0 mt-2 mr-2"
        @click="thumbnail = ''"
      >
        <trash-icon class="fill-current text-red-500" />
      </button>

      <img :src="value.content" class="max-h-56" draggable="false" />
    </template>
  </div>
</template>

<script>
import TrashIcon from "@/assets/trash.svg";

export default {
  name: "ImageEditor",
  components: {
    TrashIcon
  },
  props: {
    value: {
      required: true,
      type: Object
    }
  },
  data() {
    return {
      thumbnail: null,
      maxFileSize: 3,
      extensions: ["jpg", "jpeg", "png", "svg"]
    };
  },
  watch: {
    thumbnail(val) {
      this.$emit("change", val);
    }
  },
  methods: {
    onChange({ target }) {
      const file = target.files[0];

      if (this.checkFileType(file) && this.checkFileSize(file))
        this.setThumbnail(file);
    },
    onDrop(e) {
      const file = e.dataTransfer.files[0];

      if (this.checkFileType(file) && this.checkFileSize(file))
        this.setThumbnail(file);
    },
    setThumbnail(file) {
      if (file) {
        const reader = new FileReader();
        reader.readAsDataURL(file);

        reader.onload = e => (this.thumbnail = e.target.result);
      }
    },
    checkFileType(file) {
      const ext = file.name.split(".").pop();
      return this.extensions.includes(ext);
    },
    checkFileSize(file) {
      const size = file.size / 1024 / 1024;
      return size <= this.maxFileSize;
    }
  }
};
</script>
