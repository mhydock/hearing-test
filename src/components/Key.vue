<template>
    <div v-bind:class="[isBlackKey ? 'bkey' : 'wkey', active ? 'active' : '']"
        v-on:mousedown="onmousedown"
        v-on:mouseup="onmouseup">
        <span class="freq" v-if="showfq">{{ Math.floor(frequency*100)/100 }}</span>
        <span v-bind:class="['res', result ? 'right' : 'wrong']" v-if="showrs">
            {{ result ? "&#x2714;" : "&#x2718;" }}
        </span>

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
        showrs: Boolean,
        keyNum: Number, 
        dudKey: Boolean, 
        active: Boolean,
        broken: Boolean,
    },
    computed: {
        id: function() { return keyNames[this.keyNum>=0 ? this.keyNum%12 : 12+this.keyNum] + this.octave; },
        isBlackKey: function () { return this.id.includes('#'); },
        frequency: function() { return Math.pow(2, this.keyNum/12) * Math.pow(2, this.octave-4) * 440; },
        result: function() { return this.dudKey == this.broken; }
    },
    methods: {
        onmousedown: function() {
            if (this.active || this.showrs) {
                if (!this.dudKey) {
                    oscNode.type = this.wavefm;
                    oscNode.frequency.setValueAtTime(this.frequency, audioCtx.currentTime);
                    gainNode.gain.setValueAtTime(+this.volume, audioCtx.currentTime);
                }
                this.$emit("played", this.showrs ? this.keyNum : null);
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
    font-size: 16px;
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
    border: 1px solid black;
    background: black;
    width: 2em;
    height: 8em;
    margin-left: -1.0625em;
    margin-right: -1.0625em;
    z-index: 1000;
}

.freq {
    display: block;
    transform: rotate(-90deg);
    transform-origin: center center 0;
    text-align: left;
    position: absolute;
    left: 0;
    right: 0; 
    bottom: 1em;
}

.wkey .freq {
    line-height: 3em;
}

.bkey .freq {
    line-height: 2em;
    color: white;
}

.res {
    display: block;
    text-align: center;
    line-height: 1em;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 1em;
}

.wkey .res {
    top: 12em;
}

.bkey .res {
    top: 6em;
}

.res.right {
    color: green;
}

.res.wrong {
    color: red;
}
</style>