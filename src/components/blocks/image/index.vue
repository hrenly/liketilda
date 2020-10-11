<template>
    <div>
        <div v-if="imgLoaded">
            <img class="img" :src="image" alt="" />
        </div>
        <div v-else>
            <label for="file">Upload image</label>
            <input
                @change="fileSelected"
                type="file"
                id="file"
                ref="uploader"
            />
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    name: 'ImageBlock',
    props: {
        data: {
            type: [Object]
        }
    },
    setup(props, { emit }) {
        const uploader = ref(null);
        const imgLoaded = ref(false);
        const image = ref(null);

        const fileSelected = () => {
            const files = uploader.value.files;
            if (files.length) {
                if (files[0].type.split('/')[0] === 'image') {
                    const reader = new FileReader();
                    reader.onload = setImage;
                    imgLoaded.value = true;
                    reader.readAsDataURL(files[0]);
                }
            }
        };

        const setImage = e => {
            image.value = e.target.result;
            emit('changed', e.target.result);
        };

        return { uploader, fileSelected, imgLoaded, image };
    }
};
</script>

<style lang="stylus" scoped>
.img
    display block
    width 100%
    max-width 500px
    height auto
</style>
