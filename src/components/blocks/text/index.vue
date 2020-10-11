<template>
    <div>
        <textarea v-model="text" class="ta"></textarea>
    </div>
</template>

<script>
import { computed, onMounted } from 'vue';

export default {
    name: 'TextBlock',
    props: {
        data: {
            type: [Object]
        }
    },
    setup(props, { emit }) {
        const text = computed({
            get: () => props.data,
            set: v => emit('changed', v)
        });

        onMounted(() => {
            emit('changed', 'Some text');
        });
        return { text };
    }
};
</script>

<style lang="stylus" scoped>
.ta
    display block
    width 100%
    height 100px
    margin 0
    padding 0
    background-color #fff
    border none
    font-family Arial, Helvetica, sans-serif
    font-size 1em
    &:focus
        outline none
</style>
