<template>
    <div class="container">
        <div class="toggles">
            <button v-on:click="toggleShowFq" v-bind:class={on:showfq}>{{ showfq ? "Hide" : "Show"}} Frequencies</button>
            <button v-on:click="toggleShowOv" v-bind:class={on:showov}>{{ showov ? "Hide" : "Show"}} Octave Select</button>
            <button v-on:click="toggleShowWf" v-bind:class={on:showwf}>{{ showwf ? "Hide" : "Show"}} Waveform Select</button>
        </div>
        <div class="keyboard">
            <Key v-for="key in keys" v-bind:key="key.id"
                v-bind:octave="octave"
                v-bind:volume="volume"
                v-bind:wavefm="wavefm"
                v-bind:showfq="showfq"
                v-bind="key"/>
        </div>
        <div class="controls">
            <div v-if="showov">
                <label>Octave:</label>
                <div class="spinner">
                    <button v-on:click="octaveDn">-</button>
                    <span>{{ octave }}</span>
                    <button v-on:click="octaveUp">+</button>
                </div>
            </div>
            <div>
                <label>Volume:</label>
                <input class="slider" type="range" min="0" max=".2" step="0.001" v-model.number="volume"/>
                <span class="volPerc">{{ Math.floor(volume*500) + "%" }}</span>
            </div>
            <div v-if="showwf">
                <label>Waveform:</label>
                <select v-model="wavefm">
                    <option value="sine">Sine</option>
                    <option value="square">Square</option>
                    <option value="sawtooth">Sawtooth</option>
                    <option value="triangle">Triangle</option>
                </select>
            </div>
        </div>
    </div>
</template>

<script>
import Key from './Key.vue'

export default {
    name: "Piano",
    data() {
        return {
            octave: 4,
            volume: .2, // idk why, but vivaldi doesn't produce any audible difference beyond .2
            wavefm: "sine",
            showfq: false,
            showwf: false,
            showov: false,
            numKeys: 24,
            optionSelected: false
        }
    },
    computed: {
        keys: function () {
            var keys = [];
            for (var i = 0; i < this.numKeys; i++) {
                const k = i-4;
                const key = {
                    keyNum: k,
                    dudKey: Math.random() < 0.1,
                    active: true,
                    shared: {
                        played: false,
                        needsTuning: false,
                    }
                };
                keys.push(key);
            }
            return keys;
        }
    },
    methods: {
        toggleShowFq: function() {
            this.showfq = !this.showfq;
        },
        toggleShowWf: function() {
            this.showwf = !this.showwf; 
        },
        toggleShowOv: function() {
            this.showov = !this.showov; 
        },
        octaveUp: function() {
            if (this.octave < 8) this.octave++;
        },
        octaveDn: function() {
            if (this.octave > 0) this.octave--;
        }
    },
    components: {
        Key
    }
}
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    overflow: auto;
    width: auto;
}
.toggles {
    margin-bottom: 1em;
}
.toggles button {
    background: #eee;
    border: 1px solid #ccc;
    width: 12em;
    line-height: 2em;
    margin: .1em;
    opacity: .7;
    -webkit-transition: .2s;
    transition: opacity .2s;
}
.toggles button:hover {
    opacity: 1;
}
.toggles button.on {
    background: #35e;
    border-color: #46f;
    color: white;
}
.keyboard {
    display: flex;
    flex-direction: row;
    justify-content: center;
}
.controls {
    margin: 1em;
}
.controls div {
    display: inline-block;
}
.controls span,
.controls label, 
.controls button {
    line-height: 1em;
    display: inline-block;
    font-size: 16px;
    padding-top: 1em;
    padding-bottom: 1em;
}
.controls label {
    padding-right: 1em;
}
.spinner {
    display: inline-block;
    padding-right: 2em;
}
.spinner span,
.spinner button {
    border: 1px solid black;
    padding: 1em;
}
.spinner button {
    border-left-color: #46f;
    border-right-color: #46f;
    background: #35e;
    opacity: .7;
    -webkit-transition: .2s;
    transition: opacity .2s;
}
.spinner button:hover {
    opacity: 1;
}
.spinner button:first-child {
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
}
.spinner button:last-child {
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
}
.slider {
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
.slider:hover {
    opacity: 1;
}
.slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    background: #35e;
    border-radius: 50%;
    width: 2em;
    height: 2em;
    cursor: pointer;
}
.slider::-moz-range-thumb {
    background: #35e;
    border-radius: 50%;
    width: 2em;
    height: 2em;
    cursor: pointer;
}
.controls .volPerc {
    width: 3em; 
    text-align: center;
    padding-left: .5em;
    padding-right: 2em;
}
.controls select {
    padding: 1em;
    line-height: 1em;
}
</style>