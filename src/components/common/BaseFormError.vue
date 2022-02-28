<template>
    <p class="formError">
        <slot></slot>
    </p>
</template>

<script>
    export default {
        name: 'BaseFormError'
    };
</script>

<style>
.formError {
    color: #ff0d0d;
    text-shadow: 0 0 4px #f38b8b;
}
</style>