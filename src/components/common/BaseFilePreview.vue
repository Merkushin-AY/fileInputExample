<template>
    <div class="filePreview">
        <img class="filePreview_image" :src="src" @load="onLoad">
        <a class="filePreview_action filePreview_delete" @click="$emit('delete')">
            <svg class="filePreview_actionIcon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M0 0h24v24H0V0z" fill="none"/><path d="M18.3 5.71c-.39-.39-1.02-.39-1.41 0L12 10.59 7.11 5.7c-.39-.39-1.02-.39-1.41 0-.39.39-.39 1.02 0 1.41L10.59 12 5.7 16.89c-.39.39-.39 1.02 0 1.41.39.39 1.02.39 1.41 0L12 13.41l4.89 4.89c.39.39 1.02.39 1.41 0 .39-.39.39-1.02 0-1.41L13.41 12l4.89-4.89c.38-.38.38-1.02 0-1.4z"/></svg>
        </a>
        <a class="filePreview_action filePreview_moveLeft" @click="$emit('move', 'left')">
            <svg class="filePreview_actionIcon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M0 0h24v24H0V0z" fill="none"/><path d="M14.71 6.71c-.39-.39-1.02-.39-1.41 0L8.71 11.3c-.39.39-.39 1.02 0 1.41l4.59 4.59c.39.39 1.02.39 1.41 0 .39-.39.39-1.02 0-1.41L10.83 12l3.88-3.88c.39-.39.38-1.03 0-1.41z"/></svg>        </a>
        <a class="filePreview_action filePreview_moveRight" @click="$emit('move', 'right')">
            <svg class="filePreview_actionIcon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M0 0h24v24H0V0z" fill="none"/><path d="M9.29 6.71c-.39.39-.39 1.02 0 1.41L13.17 12l-3.88 3.88c-.39.39-.39 1.02 0 1.41.39.39 1.02.39 1.41 0l4.59-4.59c.39-.39.39-1.02 0-1.41L10.7 6.7c-.38-.38-1.02-.38-1.41.01z"/></svg>
        </a>
    </div>
</template>

<script>
    export default {
        name: 'BaseFilePreview',
        props: {
            file: {
                type: File,
                required: true,
            }
        },
        computed: {
            src: function () {
                return window.URL.createObjectURL(this.file);
            },
        },
        methods: {
            onLoad() {
                window.URL.revokeObjectURL(this.src);
            },
        },
    };
</script>

<style>
.filePreview {
    position: relative;
    aspect-ratio: 1 / 1;
    border-radius: 10px;
    overflow: hidden;
}
.filePreview_image {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
.filePreview_action {
    position: absolute;
    box-sizing: border-box;
    width: 24px;
    height: 24px;
    padding: 4px;
    background: hsla(0, 0%, 100% ,0.2);
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.3s;
    fill: #ffffff;
}
.filePreview_actionIcon {
    width: 100%;
    height: 100%;
    fill: inherit;
}
.filePreview:hover .filePreview_action {
    transform: scale(1.1);
    background: hsla(0, 0%, 100% ,0.5);
    fill: #222222;
}
.filePreview_action:hover {
    fill: #222222!important;
    background: hsla(0, 0%, 100% ,0.7)!important;
}
.filePreview_delete {
    top: 2px;
    right: 2px;
}
.filePreview_moveLeft,
.filePreview_moveRight {
    top: calc(50% - 12px);
    opacity: 0;
}
.filePreview:hover .filePreview_moveLeft,
.filePreview:hover .filePreview_moveRight {
    opacity: 1;
}
.filePreview_moveLeft {
    left: 2px;
}
.filePreview_moveRight {
    right: 2px;
}
@media (max-width: 767px) {
    .filePreview_moveLeft,
    .filePreview_moveRight {
        opacity: 1;
    }
}
</style>