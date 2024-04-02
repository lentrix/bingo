<script setup>
import { ref } from 'vue'
import { useStorage } from '@vueuse/core'

const letters = [
    {letter: 'B', color: 'blue'},
    {letter: 'I', color: 'red'},
    {letter: 'N', color: 'green'},
    {letter: 'G', color: 'brown'},
    {letter: 'O', color: 'teal'},
]

const numbers = useStorage('numbers',ref(Array.from({length:25}, (value, index)=>value=false)))

const toggle = (n) => {
    numbers.value[n] = !numbers.value[n];
}

function reset() {
    numbers.value = Array.from({length:25}, (value, index)=>value=false)
}

defineExpose({
    reset
})

</script>

<template>
    <div class="wrapper">
        <div v-for="letter in letters" :style="'background-color: ' + letter.color" class="letter">{{ letter.letter }}</div>
        <div v-for="n in 25" @click="toggle(n-1)" :class="{'selected': n!=13 && numbers[n-1]}">
            {{ n==13 ? "FREE" : "&nbsp;" }}
        </div>
    </div>
</template>


<style scoped>

* {
    font-family: Arial, Helvetica, sans-serif;
}

.letter {
    font-size: 2em;
    color: white!important;
    font-weight: bold;
}

.wrapper {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
}

.wrapper > div {
    aspect-ratio: 1/1;
    border: 3px solid #222;
    display: flex;
    align-items: center;
    justify-content: center;
    color: black;
    font-weight: bold;
    background-color: #fefefe;
    cursor: pointer;
}

.selected:after {
    content: '';
    width: 80%;
    height: 80%;
    aspect-ratio: 1/1;
    border-radius: 50%;
    background-color: red;

}



</style>