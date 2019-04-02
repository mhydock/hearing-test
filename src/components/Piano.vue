<template>
    <div class="container">
        <button v-on:click="start">Start</button>
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
                v-bind="key"
                v-on:played="played" />
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
            keys: [],
            octave: 4,
            volume: .2, // idk why, but vivaldi doesn't produce any audible difference beyond .2
            wavefm: "sine",
            showfq: false,
            showwf: false,
            showov: false,
            numKeys: 24,
            choiceMade: false,
            keyIndex: -1
        }
    },
    computed: {
        activeKey: function() {
            if (this.keyIndex < this.keys.length && this.indexes) {
                return this.keys[this.indexes[this.keyIndex]];
            }
            return null;            
        },
        keyPlayed: function() {
            return this.activeKey ? this.activeKey.played : false;
        }
    },
    watch: {
        numKeys: function() {
            this.genKeys();
        }
    },
    methods: {
        start: function () {
            this.indexes = Array(this.keys.length);
            for (var i = 0; i < this.indexes.length; i++) this.indexes[i] = i;
            this.indexes.sort(() => { return 0.5 - Math.random(); });

            this.keyIndex = 0;
            this.keys[this.indexes[this.keyIndex]].active = true;
        },
        played: function() {
            this.activeKey.played = true;
        },
        keyWorks: function() {
            this.activeKey.broken = false;
            this.nextKey();
        },
        keyBroke: function() {
            this.activeKey.broken = true;
            this.nextKey();
        },
        nextKey: function() {
            console.log(this.activeKey);
            if (this.activeKey) this.activeKey.active = false;
            this.keyIndex++;
            if (this.activeKey) this.activeKey.active = true;
        },
        genKeys: function() {
            for (var i = 0; i < this.numKeys; i++) {
                const k = i-4;
                const key = {
                    keyNum: k,
                    dudKey: Math.random() < 0.1,
                    active: false,
                    played: false,
                    broken: false,
                };
                this.keys.push(key);
            }
        }
    },
    created: function() {
        this.genKeys();
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
.controls input,
.controls label, 
.controls button,
.controls select {
    line-height: 1em;
    display: inline-block;
    vertical-align: middle;
    font-size: 16px;
}
.controls label {
    margin-left: 1em;
    margin-right: 1em;
    padding-top: 1em;
    padding-bottom: 1em;
}
.controls select {
    padding: .8em;
    line-height: 1em;
    margin-right: 1em;
}
.userChoices {
    display: flex;
    flex-direction: row;
    justify-content: center;
    padding: .5em 2em;
}
</style>