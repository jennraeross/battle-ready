<script setup>
import { ref } from "vue";

defineProps({
  msg: String,
});

const count = ref(0);
</script>

<template lang="pug">
div.p-6.bg-white.rounded-md.shadow-md.flex.flex-col.border
    slot
</template>

<style scoped>

</style>