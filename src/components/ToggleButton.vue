<template>
    <button v-on:click="$emit('input', !value)" v-bind:class={on:value}>
        {{ value ? "Hide" : "Show"}} <slot></slot>
    </button>
</template>

<script>
export default {
    name: "ToggleButton",
    props: {
        value: Boolean
    },
}
</script>

<style scoped>
button {
    background: #eee;
    border: 1px solid #ccc;
    width: 12em;
    line-height: 2em;
    margin: .1em;
    opacity: .7;
    -webkit-transition: .2s;
    transition: opacity .2s;
}
button:hover {
    opacity: 1;
}
button.on {
    background: #35e;
    border-color: #46f;
    color: white;
}
</style>