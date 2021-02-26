<template>
  <div>
    <div class="mb-2">
      <legend class="text-base font-bold text-gray-900">
        Type:
      </legend>
    </div>

    <div class="flex items-center mb-3">
      <div class="flex items-center mr-6">
        <input
          id="text"
          v-model="data"
          name="type"
          value="text"
          type="radio"
          class="focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-300"
        />
        <label for="text" class="ml-3 block text-base text-gray-700">
          Text
        </label>
      </div>
      <div class="flex items-center">
        <input
          id="image"
          v-model="data"
          name="type"
          value="image"
          type="radio"
          class="focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-300"
        />
        <label for="image" class="ml-3 block text-base text-gray-700">
          Image
        </label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TypeSelector",
  props: {
    value: {
      required: true,
      type: String
    }
  },
  data() {
    return {
      data: ""
    };
  },
  watch: {
    value(val) {
      this.data = val;
    }
  },
  created() {
    this.data = this.value;
    this.$watch("data", value => this.$emit("input", value));
  }
};
</script>
