<script setup>

const props = defineProps({
    number: Number,
    drawn: Boolean
})

</script>

<template>

<div :class="{'drawn': drawn}">

    {{ number }}

</div>


</template>

<style scoped>
div {
    border-radius: 50%;
    aspect-ratio: 1/1;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 80%;
    height: 80%;
}
.drawn {
    background-color: yellow;
    color: black;
    font-weight: bold;
}

</style>