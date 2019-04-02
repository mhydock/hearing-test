<template>
    <div class="slider">
        <label v-if="label">{{ label }}:</label>
        <input type="range" :min="min" :max="max" :step="step" v-model="value"/>
        <label class="percent">{{ Math.floor((value-min)/(max-min)*100) + "%" }}</label>
    </div>
</template>

<script>
export default {
    name: "Slider",
    props: {
        label: String,
        value: Number,
        min: Number,
        max: Number,
        step: Number
    }
}
</script>

<style>
.slider * {
    display: inline-block;
}
.slider input {
    -webkit-appearance: none;  /* Override default CSS styles */
    appearance: none;
    background: #d3d3d3;
    border-radius: 1em;
    outline: none;

    width: 10em;
    height: 1em;
    margin-top: 1em;
    margin-bottom: 1em;
    opacity: 0.7; 
    -webkit-transition: .2s;
    transition: opacity .2s;

    cursor: pointer;
}
.slider input:hover {
    opacity: 1;
}
.slider input::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    background: #35e;
    border-radius: 50%;
    width: 2em;
    height: 2em;
    cursor: pointer;
}
.slider input::-moz-range-thumb {
    background: #35e;
    border-radius: 50%;
    width: 2em;
    height: 2em;
    cursor: pointer;
}
.slider .percent {
    width: 3em; 
    text-align: center;
}
</style>