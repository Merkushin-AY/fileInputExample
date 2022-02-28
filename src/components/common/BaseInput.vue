<template>
    <input :type="type" :name="name" @change="$emit('change', $event)">
</template>

<script>
    export default {
        name: 'BaseInput',
        props: {
            type: {
                type: String,
                default: 'text',
            },
            name: {
                type: String,
                default: '',
            }
        }
    };
</script>

<style>

</style>