<template>
    <h1>draggable component</h1>
    <button @click="lianxian" :style="{color: lianxianBegin?'green' : 'black'}">点击开始连线</button>
    <hr>
    <Draggable :items="items" :lianxianBegin="lianxianBegin" @closeLianxian="closeLianxian">
        <template #default="{ data }">
            <DraggableItem>
                <Anything :data="data"></Anything>
            </DraggableItem>
        </template>

    </Draggable>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import Anything from './components/Anything.vue';
import Draggable from './components/Draggable.vue';
import DraggableItem from './components/DraggableItem.vue';
const items = [
    {
        id: 1,
        text: 'item 1',

    },
    {
        id: 2,
        text: 'item 2',
    },
    {
        id: 3,
        text: 'item 3',
    },
];
let lianxianBegin = ref(false);
function lianxian(e) {
    if (!lianxianBegin.value) {
        console.log('连线开始...');
        lianxianBegin.value = true;
    } else {
        console.log('连线结束...');
        lianxianBegin.value = false;
    }
}
function closeLianxian() {
    console.log('连线关闭!');
    
    lianxianBegin.value = false;
}
</script>
<style scoped></style>
