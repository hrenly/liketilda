<template>
    <div>
        <div class="controls">
            <div class="controls__wrap">
                <div @click="action('up')" class="controls__item up"></div>
                <div @click="action('down')" class="controls__item down"></div>
                <div
                    @click="action('remove')"
                    class="controls__item remove"
                ></div>
            </div>
        </div>
        <component :is="item.type" />
    </div>
</template>

<script>
// import { ref } from 'vue';
import * as blocks from '@/components/blocks';

let comps = {};
Object.keys(blocks).forEach(el => {
    comps[el] = blocks[el];
});

export default {
    name: 'pageItem',
    components: {
        ...comps
    },
    props: {
        item: Object
    },
    setup(props, { emit }) {
        const action = type => emit(type, props.item.id);
        return { action };
    }
};
</script>

<style lang="stylus" scoped>
.controls
    float right
    position relative
    &__wrap
        position absolute
        width 60px
    &__item
        display inline-block
        width 20px
        height 20px
    .remove
        background-color red
    .up
        background-color blue
    .down
        background-color green
</style>
