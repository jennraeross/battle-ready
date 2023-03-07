<script setup>
import { ref } from "vue";
</script>

<template lang="pug">
button.border.rounded.px-6.py-2.bg-gray-50.border(class="hover:bg-gray-100 active:border-2")
    slot
</template>

<style scoped>

</style>