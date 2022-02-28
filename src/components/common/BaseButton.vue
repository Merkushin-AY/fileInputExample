<template>
    <button class="button">
        <slot></slot>
    </button>
</template>

<script>
    export default {
        name: 'BaseButton'
    };
</script>

<style scoped>
.button {
    text-align: center;
    padding: 20px;
    color: #ffffff;
    background: hsla(0, 0%, 100% ,0.2);
    border: 2px solid hsla(0, 0% ,100%, 0.1);
    border-radius: 10px;
    margin-top: 24px;
    cursor: pointer;
    transition: background-color .3s ease;
    text-transform: uppercase;
}
.button:hover {
    background-color: hsla(0,0%,100%,.4);
}
</style>