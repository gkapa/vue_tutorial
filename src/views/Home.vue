<template>
  <Home />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Home from "@/components/Home.vue"; // @ is an alias to /src

export default defineComponent({
  components: {
    Home,
  },
});
</script>
