<script setup>
import { ref } from "vue";
import Card from "./card.vue"

defineProps({
  msg: String,
});

const count = ref(0);
</script>

<template lang="pug">
.display-flex.flex-row.flex-nowrap
    .basis-10.bg-white
        Card Testing 1
    .basis-32.bg-black
        Card Testing 2
    .basis-auto.display-flex.flex-col
        Card Testing 3
        Card Testing 4
</template>

<style scoped>

</style>
