<template>
    <div
        class="fileFormExample"
    >
        <BaseForm
            url="/test-api"
        >
            <BaseFileInput name="pictures" accept=".jpg,.jpeg,.png" :max-file-size="100000000"/>
            <BaseButton class="fileFormExample_button" type="submit">Submit</BaseButton>
        </BaseForm>
    </div>
</template>

<script>
    import BaseForm from '@/components/common/BaseForm'
    import BaseFileInput from '@/components/common/BaseFileInput'
    import BaseButton from '@/components/common/BaseButton'

    export default {
        name: 'FileFormExample',
        components: {
            BaseForm,
            BaseFileInput,
            BaseButton,
        }
    };
</script>

<style>
.fileFormExample {
    display: flex;
    box-sizing: border-box;
    max-width: 640px;
    padding: 24px;
    margin: 0 auto;
    border: 1px solid rgba(255, 255, 255, 0.27);
    background: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
    backdrop-filter: blur(9.3px);
    -webkit-backdrop-filter: blur(9.3px);
}
.fileFormExample_button {
    width: 100%;
}
</style>