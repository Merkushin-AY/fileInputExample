<template>
    <form class="form" @submit="onSubmit($event)" ref="form">
        <slot></slot>
        <BaseFormError v-for="(errorText, idx) in errors" :key="idx">
            {{ errorText }}
        </BaseFormError>
    </form>
</template>

<script>
    import BaseFormError from '@/components/common/BaseFormError';


    /**
     * Основной компонент для создания формы. Обрабатывает и отсылает запросы
     */
    export default {
        name: 'BaseForm',
        components: {
            BaseFormError
        },
        data: () => ({
            errors: [],
        }),
        props: {
            url: {
                type: String,
                required: true,
            },
            method: {
                type: String,
                default: 'POST',
            },
        },
        methods: {
            onSubmit(e) {
                e.preventDefault();
                this.errors = [];

                const form = this.$refs.form;

                /**
                 * Тут должна быть повторная валидация всех полей
                 */

                const formData = new FormData(form);

                this.addChildrenFiles(formData)

                const fetchOptions = {
                    method: this.method,
                    body: formData,
                };

                fetch(this.url, fetchOptions).then(response => {
                    if (response.status !== 200) throw new Error('Wrong status');
                    // Успех

                }).catch(error => {
                    this.errors.push('Что-то пошло не так')
                });
            },

            /**
             * Функция, которая собирает файлы с дочерних элементов
             * Такой подход хорош тем, что не придётся связывать элементы через родительский компонент, форма сама автоматически соберет все файлы со всех файл инпутов сколько бы их не было
             * Но следует понимать, что такая реализация накладывает ограничения на вложенность файл инпута, то есть если он будет внутри другого компонента, то данный способ не сработает
             * в таком случае можно применить рекурсивный обход или обязать обертку файл инпутов тоже иметь такой метод
             * так же возможно третье решение в виде стороннего хранилища состояний (в каком-то общем объекте)
             * @param formData
             * @returns {*}
             */
            addChildrenFiles(formData) {
                this.$slots.default.forEach(slot => {
                    if (!slot.componentInstance?.getFiles) return false

                    const {name, files} = slot.componentInstance.getFiles();
                    files.forEach((file) => formData.append(name, file))
                });
                return formData;
            },
        }
    };
</script>

<style>
.form {
    width: 100%;
}
</style>