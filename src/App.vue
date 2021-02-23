<template>
  <div class="h-screen flex">
    <!-- preview -->
    <preview />
    <!-- /preview -->

    <!-- editor -->
    <editor />
    <!-- /editor -->
  </div>
</template>

<script>
import Preview from "@/components/Preview/index";
import Editor from "@/components/Editor/index";

export default {
  name: "App",
  components: {
    Preview,
    Editor
  }
};
</script>
