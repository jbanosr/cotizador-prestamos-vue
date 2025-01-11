<script setup>
    //const props = defineProps(['operador']);
    const props = defineProps({
        operador: String,
    });
</script>

<template>
    <button
        type="button"
        class="h-10 w-10 flex items-center justify-center font-bold text-white text-2xl bg-lime-500 rounded-full hover:outline-none hover:ring-2 hover:rling-offset-2 hover:ring-lime-500"
        @click="$emit('fn')"
    >{{ operador }}</button>
 </template>