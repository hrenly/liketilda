<template>
    <div class="wrap">
        <ul class="list">
            <li
                v-for="block in blocksList"
                :key="block.__hmrId"
                @click="addItem(block.name)"
                class="list__item"
            >
                {{ block.name }}
            </li>
        </ul>
        <div class="page">
            <div v-for="item in page" :key="item.id">
                <page-item
                    :item="item"
                    @remove="removeItem"
                    @up="upItem"
                    @down="downItem"
                />
            </div>
        </div>
    </div>
</template>

<script>
import { computed, ref } from 'vue';
import * as blocks from '@/components/blocks';
import pageItem from './item';

let comps = {};
Object.keys(blocks).forEach(el => {
    comps[el] = blocks[el];
});

export default {
    name: 'Page',
    components: {
        pageItem
    },
    setup() {
        const page = ref([]);

        const blocksList = computed(() => comps);

        const getId = () =>
            Number.parseInt(Date.now() * Math.random(), 10)
                .toString()
                .slice(-6);
        const addItem = type =>
            page.value.push({
                id: getId(),
                type
            });
        const getIndex = id => page.value.findIndex(el => el.id === id);
        const removeItem = id => page.value.splice(getIndex(id), 1);
        const upItem = id => {
            const from = getIndex(id);
            moveItem(from, from + 1);
        };
        const downItem = id => {
            const from = getIndex(id);
            moveItem(from, from - 1);
        };
        const moveItem = (from, to) => {
            page.value.splice(to, 0, page.value.splice(from, 1)[0]);
        };

        return { blocksList, page, addItem, removeItem, upItem, downItem };
    }
};
</script>

<style lang="stylus" scoped>
.wrap
    display flex
.list
    width 200px
    margin 0
    padding 0
    &__item
        display block
        margin 0
        padding 2px 4px
        border 1px solid #999
        border-bottom none
        &:last-child
            border-bottom 1px solid #999
</style>
