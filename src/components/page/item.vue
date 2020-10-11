<template>
    <div>
        <div class="controls">
            <div class="controls__wrap">
                <div @click="action('up')" class="controls__item up">
                    &#5121;
                </div>
                <div @click="action('down')" class="controls__item down">
                    &#5123;
                </div>
                <div @click="action('remove')" class="controls__item remove">
                    &#7413;
                </div>
            </div>
        </div>
        <component :is="item.type" :data="item.data" @changed="change" />
    </div>
</template>

<script>
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
        const change = data => {
            emit('update', { id: props.item.id, data });
        };

        return { action, change };
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
        font-size 14px
        text-align center
        line-height 20px
        vertical-align top
        color white
    .remove
        background-color red
        font-size 20px
    .up
    .down
        background-color blue
</style>
