<template>
  <aside class="w-2/6 bg-mango-400 transform rotate-6 p-24 -mb-14 -mt-10">
    <div
      class="transform -rotate-6 text-2xl h-full flex justify-center items-center"
    >
      <h1
        v-if="area === 0"
        class="text-gray-800 text-6xl font-bold text-center"
      >
        Please select one of the <u>area</u>s
      </h1>

      <area-editor :area="area" v-else />
    </div>
  </aside>
</template>

<script>
import eventBus from "@/eventBus";
import AreaEditor from "./components/AreaEditor/index";

export default {
  name: "Editor",
  components: {
    AreaEditor
  },
  data() {
    return {
      area: 0
    };
  },
  mounted() {
    eventBus.$on("selectArea", payload => (this.area = payload));
  }
};
</script>
