<script setup>
import { onMounted, ref, useTransitionState } from 'vue'
import {RouterLink} from 'vue-router'
import Number from '@/components/Number.vue'
import { useStorage } from '@vueuse/core'
import Pattern from '@/components/Pattern.vue'


// const drawnNumbers = ref([4,8,10])
// const availableNumbers = ref(Array.from({length: 75}, (value, index)=> value = index+1))

const drawnNumbers = useStorage('drawnNumbers', [])
const availableNumbers = useStorage('availableNumbers', Array.from({length: 75},(value,index)=>value=index+1))
const current = ref(Number)
const currentLetter = ref('')

const drawNext = () => {
    const i = Math.floor((Math.random() * availableNumbers.value.length));

    const num = availableNumbers.value[i]

    drawnNumbers.value.push(num);
    availableNumbers.value.splice(i,1);

    currentLetter.value = getLetter(num)

}

const pattern = ref()

const getLetter = (num) => {
    if(num <= 15) return 'B'
    else if(num<=30) return 'I'
    else if(num<=45) return 'N'
    else if(num<=60) return 'G'
    else if(num<=75) return 'O'
}

const resetGame = () => {
    const res = confirm("Are you sure you want to reset the game now?")
    if(res) {
        drawnNumbers.value = []
        availableNumbers.value = Array.from({length: 75},(value,index)=>value=index+1)
        pattern.value.reset()
        currentLetter.value = ''
    }
}

</script>

<template>
  
  <div class="container">

    <div class="sidebar">
        <img alt="Vue logo" class="logo" src="@/assets/mdc_logo.png" />
        <div id="controls">
            <button style="background-color: green;" @click="drawNext">Next Draw</button>
            <button style="background-color: red;" @click="resetGame">Reset Game</button>
        </div>
        <h3>Pattern</h3>
        
        <Pattern ref="pattern" />

    </div>

    <div class="content">
        <h3 id="number-board-heading" style="color: #fff">Number Board</h3>
        <div style="display: grid; grid-template-columns: repeat(16, 1fr); grid-template-rows: auto;">
            <!-- B line -->
            <div class="letter" style="background-color: blue;">
                <div>B</div>
            </div>
            <div class="number" v-for="num in 15"><Number :number="num" :drawn="drawnNumbers.includes(num)"/></div>

            <!-- I line -->
            <div class="letter" style="background-color: red;">
                <div>I</div>
            </div>
            <div class="number" v-for="num in 15"><Number :number="num+15" :drawn="drawnNumbers.includes(num+15)"/></div>

            <!-- N line -->
            <div class="letter" style="background-color: green;">
                <div>N</div>
            </div>
            <div class="number" v-for="num in 15"><Number :number="num+30" :drawn="drawnNumbers.includes(num+30)"/></div>

            <!-- G line -->
            <div class="letter" style="background-color: brown;">
                <div>G</div>
            </div>
            <div class="number" v-for="num in 15"><Number :number="num+45" :drawn="drawnNumbers.includes(num+45)"/></div>

            <!-- O line -->
            <div class="letter" style="background-color: teal;">
                <div>O</div>
            </div>
            <div class="number" v-for="num in 15"><Number :number="num+60" :drawn="drawnNumbers.includes(num+60)"/></div>

        </div>
        
        <div class="status">
            <div class="current-draw">
                <h4>Current Draw</h4>
                <div>
                    <div id="current-letter">{{ currentLetter }}</div>
                    <div id="current-number">{{ drawnNumbers[drawnNumbers.length-1] }}</div>
                </div>
            </div>
            <div class="previous-draws">
                <h4>Previous Draws</h4>
                <div>
                    <div v-for="n in 4">
                        <div class="previous-letter">{{ getLetter(drawnNumbers[drawnNumbers.length-n-1]) }}</div>
                        <div class="previous-number">{{ drawnNumbers[drawnNumbers.length-n-1] }}</div>
                    </div>
                </div>
            </div>
        </div>
    </div>

  </div>

</template>

<style>

.container {
    display: grid;
    grid-template-columns: 2fr 7fr;
    height: 100vh;
}

.sidebar {
    background-color: #ccaaaa;
    padding-inline: 1rem;
}

.sidebar > img {
    width: 220px;
    height: 220px;
    margin: 2rem auto;
    display:block;
}

.sidebar > nav {
    display: flex;
}

.sidebar > nav > a {
    flex: 1;
    text-align: center;
    padding: 1rem;
    background-color: #520000;
    text-decoration: none;
    color: #efefef;
    font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}

.sidebar > nav > a:hover {
    background-color: #111;
}

.content {
    background-color: #250000;
    padding: 1rem 3rem;
}

h3 {
    font-size: 2.5em;
    font-weight: bold;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    text-align: center;
    padding-block: 1rem;
}

.number,
.letter {
    aspect-ratio: 1/1;
    border: 1px solid #ddd;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 2.5em;
    background-color: #111;
}

.number {
    color: #555;
}

.letter {
    color: white;
}

#controls {
    margin-top: 2rem;
    display: flex;
    gap: 5px;
}

#controls > button {
    font-size: 1.3em;
    padding: 1rem 2rem;
    color: white;
    border-radius: 1rem;
    flex: 1;
}

#controls > button:hover {
    background-color: white!important;;
    color: red;
}

.status {
    margin-top: 2rem;
    display: grid;
    grid-template-columns: 1fr 5fr;
    gap: 1rem;
}

.current-draw > h4 {
    font-size: 2rem;
    color: white;
    text-align: center;
}

.current-draw > div{
    margin-top: 1rem;
    width: 350px;
    height: 350px;
    border-radius: 50%;
    border: 2rem solid brown;
    background-color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    font-weight: bold;
    font-family: Arial, Helvetica, sans-serif;
}
.current-draw > div > #current-letter {
    margin-top: -12px;
    font-size: 5em;
}
.current-draw > div > #current-number {
    font-size: 14em;
    margin-top: -28px;
}

.previous-draws {
    border: 2px solid #fefefe;
    border-radius: 2rem;
    padding: 1rem;
}

.previous-draws > h4 {
    color: white;
    font-size: 2rem;
    text-align: center;
    
}

.previous-draws > div {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    padding: 1rem;
    gap: 2rem;
}

.previous-draws > div > div {
    aspect-ratio: 1/1;
    border-radius: 50%;
    border: 1rem solid red;
    background-color: yellow;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

.previous-draws > div > div > .previous-letter {
    font-size: 3em;
}

.previous-draws > div > div > .previous-number {
    font-size: 6em;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: 700;
}

</style>
