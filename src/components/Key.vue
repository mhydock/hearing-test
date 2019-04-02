<template>
    <div v-bind:class="[isBlackKey ? 'bkey' : 'wkey', active ? 'active' : '']"
        v-on:mousedown="onmousedown"
        v-on:mouseup="onmouseup">
        <span v-if="showfq">{{ Math.floor(frequency*100)/100 }}</span>
    </div>
</template>

<script>
const keyNames = ['A', 'A#', 'B', 'C', 'C#', 'D', 'D#' , 'E', 'F', 'F#', 'G', 'G#'];
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
const oscNode = audioCtx.createOscillator();
const gainNode = audioCtx.createGain();

gainNode.gain.value = 0;
oscNode.type = "square";

oscNode.connect(gainNode);
gainNode.connect(audioCtx.destination);

oscNode.start();

export default {
    name: 'Key',
    props: {
        octave: Number,
        volume: Number,
        wavefm: String,
        showfq: Boolean,
        keyNum: Number, 
        dudKey: Boolean, 
        active: Boolean,
    },
    computed: {
        id: function() { return keyNames[this.keyNum>=0 ? this.keyNum%12 : 12+this.keyNum] + this.octave; },
        isBlackKey: function () { return this.id.includes('#'); },
        frequency: function() { return Math.pow(2, this.keyNum/12) * Math.pow(2, this.octave-4) * 440; },
    },
    methods: {
        onmousedown: function() {
            if (this.active) {
                if (!this.dudKey) {
                    oscNode.type = this.wavefm;
                    oscNode.frequency.setValueAtTime(this.frequency, audioCtx.currentTime);
                    gainNode.gain.setValueAtTime(+this.volume, audioCtx.currentTime);
                }
                this.$emit("played", this.keyNum);
            }
        },
        onmouseup: function() {
            gainNode.gain.setValueAtTime(0, audioCtx.currentTime);
        }
    }
}
</script>

<style scoped>
.wkey, .bkey {
    margin: 1px;
    vertical-align: top;
    overflow: auto;
    position: relative;
    cursor: default;
    -webkit-transition: .2s; /* 0.2 seconds transition on hover */
    transition: background-color .2s;
}

.wkey.active:hover,
.bkey.active:hover {
    background: orange;
}

.wkey.active,
.bkey.active {
    background: #ffc250;
    cursor: pointer;
}

.wkey {
    border: 1px solid black;
    background: white;
    width: 3em;
    height: 14em;
}

.bkey {
    background: black;
    width: 2em;
    height: 8em;
    margin-left: -1em;
    margin-right: -1em;
    z-index: 1000;
}

.wkey > span,
.bkey > span {
    display: block;
    transform: rotate(-90deg);
    transform-origin: center center 0;
    text-align: left;
    position: absolute;
    left: 0;
    right: 0; 
    bottom: 1em;
}

.wkey > span {
    line-height: 3em;
}

.bkey > span {
    line-height: 2em;
    color: white;
}
</style>