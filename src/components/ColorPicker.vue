<template>
    <div class="container">
        <h2>Colour Picker</h2>
        <div class="picker">
            <label for="primaryColour">Primary</label>
            <input id="primaryColour" class="pickerInput" type="color" v-model="currentInputValue"
                @change="setPrimary(currentInputValue)">
            <span>{{ currentInputValue }}</span>
        </div>
        <ul>
            <li v-for="color in presetColors" :key="color.primary">
                <div class="preset" @click="setPrimary(color.primary)" :style="{backgroundColor:`${color.primary}`}">
                    <span :style="{color: calculateBrightness(`${color.primary}`)}">{{color.primary}}</span>
                </div>
            </li>
        </ul>
    </div>
</template>

<script setup lang="ts">
import { ref} from 'vue';

const root = ref<HTMLStyleElement>(document.querySelector(':root') as HTMLStyleElement);
const primaryColour = ref<string>('');
const currentInputValue = ref<string>('#000000');

//TODO maybe simplify this to a normal array
const presetColors = [
    {primary: '#B25BFD'},
    {primary: '#28FF00'},
    {primary: '#FF10F0'},
    {primary: '#1F51FF'},
    {primary: '#CFFF04'},
    {primary: '#FF073A'},
];

function setPrimary(primary: string) {
    if (root.value) {
        root.value.style.setProperty('--color-primary', primary);
        root.value.style.setProperty('--color-primary-shadow', primary + "44");
    }
}

//TODO this should come from session storage or something in case the user changes the colour.
function getPrimary(){
    if (root.value) {
        let rs = getComputedStyle(root.value);
        primaryColour.value = rs.getPropertyValue('--color-primary');
    }
}

function calculateBrightness(color: string) {
  const r = parseInt(color.slice(1, 3), 16);
  const g = parseInt(color.slice(3, 5), 16);
  const b = parseInt(color.slice(5, 7), 16);

  // https://codepen.io/WebSeed/pen/pvgqEq 
  // https://stackoverflow.com/questions/1855884/determine-font-color-based-on-background-color
  const brightness = 1 - (0.299 * r + 0.587 * g + 0.114 * b) / 255;
  

  return brightness > 0.5 ? 'white' : 'black';
}

getPrimary();

</script>

<style scoped>
.container {
    height: auto;
    width: 250px;
    margin: auto;
    padding: 1rem;
    border: solid 1px var(--color-primary);
    box-shadow: var(--shadow);
    background-color: #222;
    border-radius: 10px;
}

.picker {
    margin: 1rem auto;

    span {
        font-size: 24px;
        margin: 0.3rem;
    }
}

.pickerInput {
    width: 25%;
    height: 30px;
    border: solid 1px #ccc;
    box-shadow: var(--shadow);
    background-color: #fff;
    text-align: center;
    margin: 0.5rem auto;
}

label {
    display: block;
}

ul {
    list-style-type: none;
    padding: 0px;
    margin: auto;
}

li {
    padding-bottom: 5px;
}
</style>
