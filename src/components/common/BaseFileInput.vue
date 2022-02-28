<template>
    <div class="fileInput">
        <h3>Load your images</h3>
        <label
            class="fileInput_label"
            :class="{dragged: isDraggedOn}"
            ref="label"
        >
            <!-- Тут лучше использовать svg спрайт -->
            <svg class="fileInput_uploadImage" xmlns="http://www.w3.org/2000/svg"viewBox="0 0 24 24"><g><rect fill="none" height="24" width="24"/></g><g><path d="M7.4,10h1.59v5c0,0.55,0.45,1,1,1h4c0.55,0,1-0.45,1-1v-5h1.59c0.89,0,1.34-1.08,0.71-1.71L12.7,3.7 c-0.39-0.39-1.02-0.39-1.41,0L6.7,8.29C6.07,8.92,6.51,10,7.4,10z M5,19c0,0.55,0.45,1,1,1h12c0.55,0,1-0.45,1-1s-0.45-1-1-1H6 C5.45,18,5,18.45,5,19z"/></g>
            </svg>
            <p class="fileInput_labelDraggedText">
                Put your file slowly. It's a glass
            </p>
            <BaseInput class="fileInput_input" type="file" :accept="accept" @change="onInputChange" multiple/>
        </label>
        <BaseFormError v-for="(errorText, idx) in errors" :key="idx">
            {{ errorText }}
        </BaseFormError>
        <TransitionGroup
            name="list-up"
            tag="div"
            class="fileInput_previews"
        >
            <BaseFilePreview
                v-for="(file, index) in files"
                :key="file.fileId"
                class="fileInput_preview"
                :file="file"
                @delete="deleteFile(index)"
                @move="moveFile(index, $event)"
            />
        </TransitionGroup>
    </div>
</template>

<script>
    import BaseInput from '@/components/common/BaseInput';
    import BaseFilePreview from '@/components/common/BaseFilePreview';
    import BaseFormError from '@/components/common/BaseFormError';

    /**
     * Компонент выводит файловый инпут и превьюшки загруженных файлов
     * Содержит всю логику загрузки, удаления, перемещения файлов
     */
    export default {
        name: 'BaseFileInput',
        components: {
            BaseInput,
            BaseFilePreview,
            BaseFormError,
        },
        props: {
            name: {
                type: String,
                required: true
            },
            accept: {
                type: String,
                default: '',
            },
            maxFileSize: {
                type: Number,
                default: 0,
            }
        },
        data: () => ({
            reader: null,
            files: [],
            errors: {},
            isDraggedOn: false,
        }),
        computed: {
            formattedMaxFileSize() {
                return this.formatSize(this.maxFileSize);
            },
        },
        mounted() {
            this.$refs.label.addEventListener("dragenter", (e) => {
                e.stopPropagation();
                e.preventDefault();
                this.isDraggedOn = true;
            }, false);
            this.$refs.label.addEventListener("dragleave", (e) => {
                e.stopPropagation();
                e.preventDefault();
                this.isDraggedOn = false;
            }, false);
            this.$refs.label.addEventListener("dragover", (e) => {
                e.stopPropagation();
                e.preventDefault();
                this.isDraggedOn = true;
            }, false);
            this.$refs.label.addEventListener("drop", (e) => {
                e.stopPropagation();
                e.preventDefault();
                this.isDraggedOn = false;

                const files = e.dataTransfer.files;

                this.handleFiles(files);
            }, false);
        },
        methods: {
            onInputChange(event) {
                this.errors = {}
                this.handleFiles(event.target.files);
                event.target.value = '';
            },

            handleFiles(files) {
                // Создаем идентификатор и сохраняем в массив каждый файл прошедший валидацию
                for (const file of files) {
                    file.fileId = this.createFileId(file);

                    const fileExist = this.files.some((item) => item.fileId === file.fileId);
                    if (fileExist) continue;

                    if (!this.checkFileSize(file)) {
                        this.$set(this.errors, 'maxSize', 'Максимальный размер файла: ' + this.formattedMaxFileSize);
                        continue;
                    }

                    /**
                     * @TODO: Сюда можно добавить иные виды валидации (обязательность, количество файлов, формат файлов)
                     */


                    this.files.push(file);
                }
                this.$emit('filesChange', this.files);
            },

            createFileId(file) {
                return `${file.name}-${file.size}-${file.lastModified}`;
            },

            /**
             * Форматирвоание размера (такие функции должны быть в каких-нибдуь helpers)
             */
            formatSize(size) {
                let i = 0;
                let type = ['Б','КБ','МБ','ГБ','ТБ','ПБ'];
                while((size / 1000 | 0) && i < type.length - 1) {
                    size /= 1024;
                    i++;
                }
                return size.toFixed(2) + ' ' + type[i];
            },

            checkFileSize(file) {
                return !this.maxFileSize || file.size <= this.maxFileSize
            },

            deleteFile(fileIndex) {
                this.files.splice(fileIndex, 1);
            },

            moveFile(fileIndex, direction) {
                direction = (direction === 'right') ? 1 : -1;
                let siblingIndex = fileIndex + direction;
                let sibling = this.files[siblingIndex];
                if (sibling) {
                    this.$set(this.files, siblingIndex, this.files[fileIndex]);
                    this.$set(this.files, fileIndex, sibling);
                }
            },


            /**
             * Метод для вызова снаружи
             */
            getFiles() {
                return {
                    name: this.name,
                    files: this.files,
                }
            },
        }
    };
</script>

<style>
.fileInput_input {
    display: none;
}
.fileInput_label {
    position: relative;
    display: flex;
    flex-flow: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    aspect-ratio: 2 / 1;
    max-height: 320px;
    border-radius: 10px;
    border: 2px solid hsla(0, 0% ,100%, 0.1);
    background: hsla(0, 0%, 100% ,0.2);
    transition: background-color .3s ease;
    cursor: pointer;
}
.fileInput_label:hover,
.fileInput_label.dragged {
    background-color: hsla(0, 0%, 100%, 0.4);
}
.fileInput_labelDraggedText {
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    opacity: 0;
    margin: 0;
    transition: opacity 0.6s;
}
.fileInput_label.dragged .fileInput_labelDraggedText {
    opacity: 1;
}
.fileInput_uploadImage {
    fill: #ffffff;
    height: 100%;
    border-radius: 10px;
}
.fileInput_previews {
    display: flex;
    flex-flow: row wrap;
    gap: 16px;
    margin-top: 16px;
}
.fileInput_preview {
    width: calc(25% - 12px);
}
@media (max-width: 767px) {
    .fileInput_preview {
        width: calc(33.3% - 11px);
    }
}
@media (max-width: 500px) {
    .fileInput_preview {
        width: calc(50% - 8px);
    }
}
</style>