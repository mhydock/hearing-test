<template>
    <div class="spinner">
        <label v-if="label">{{ label }}:</label>
        <div class="wrapper">
            <button v-on:click="down">-</button>
            <span>{{ value }}</span>
            <button v-on:click="up">+</button>
        </div>
    </div>
</template>

<script>
export default {
    name: "Spinner",
    props: {
        label: String,
        value: Number,
        max: Number,
        min: Number,
        step: {
            type: Number,
            default: 1
        }
    },
    methods: {
        up: function() {
            if (this.value < this.max) this.$emit('input', this.value+this.step);
        },
        down: function() {
            if (this.value > this.min) this.$emit('input', this.value-this.step);
        },
    }
}
</script>

<style scoped>
.spinner {
    display: inline-block;
    padding-right: 2em;
}
.spinner .wrapper {
    display: inline-block;
    vertical-align: middle;
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
.spinner .wrapper button:first-child {
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
}
.spinner .wrapper button:last-child {
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
}
</style>