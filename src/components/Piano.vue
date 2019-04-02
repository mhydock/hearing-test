<template>
    <div class="container">
        <div class="toggles">
            <ToggleButton v-model="showfq">Frequencies</ToggleButton>
            <ToggleButton v-model="showov">Octave Select</ToggleButton>
            <ToggleButton v-model="showwf">Waveform Select</ToggleButton>
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
            <Spinner v-if="showov" v-model="octave" :min="0" :max="8" label="Octave"/>
            <Slider v-model="volume" :min="0" :max="0.2" :step="0.01" label="Volume"/>
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
        <div class="userChoices">
            <BigButton :disabled="!keyPlayed" :click="keyWorks" title="This key works" subtitle="I can hear the tone" />
            <BigButton :disabled="!keyPlayed" :click="keyBroke" title="This key is broken" subtitle="I cannot hear the tone" />
        </div>
    </div>
</template>

<script>
import BigButton from './BigButton.vue'
import Key from './Key.vue'
import Slider from './Slider.vue'
import Spinner from './Spinner.vue'
import ToggleButton from './ToggleButton.vue'


export default {
    name: "Piano",
    components: {
        BigButton,
        Key,
        Slider,
        Spinner,
        ToggleButton
    },
    data() {
        return {
            octave: 4,
            volume: .2, // idk why, but vivaldi doesn't produce any audible difference beyond .2
            wavefm: "sine",
            showfq: false,
            showwf: false,
            showov: false,
            numKeys: 24,
            keyPlayed: false,
            choiceMade: false,
            activeKey: {}
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
        keyWorks: function() {

        },
        keyBroke: function() {

        }
    }
}
</script>

<style>
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
}
.controls label {
    margin-left: 1em;
    margin-right: 1em;
    padding-top: 1em;
    padding-bottom: 1em;
}
.controls select {
    padding: 1em;
    line-height: 1em;
}
.userChoices {
    display: flex;
    flex-direction: row;
    justify-content: center;
    padding: .5em 2em;
}
</style>