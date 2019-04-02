<template>
    <button v-bind:disabled="disabled" v-on:click="click">
        <span>{{ title }}</span>
        <span>({{ subtitle}})</span>
    </button>
</template>

<script>
export default {
    name: "BigButton",
    props: {
        disabled: Boolean,
        click: Function,
        title: String,
        subtitle: String,
    }
}
</script>

<style scoped>
button {
    background: orange;
    border: none;
    color: white;

    width: 25em;
    height: auto;
    padding: .5em 1em;
    margin-left: 2em;
    margin-right: 2em;
    text-align: center;

    cursor: pointer;
}
button:disabled {
    background: grey;
    color: #aaa;
}
button span:first-child {
    text-transform: uppercase;
    font-size: 2em;
}
button span {
    display: block;
    width: 100%;
}
</style>