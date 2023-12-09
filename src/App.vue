<template>
    <h1>draggable component</h1>
    <hr>
    <Draggable :items="items">
        <template #default="{ data }">
            <DraggableItem>
                <Anything :data="data"></Anything>
            </DraggableItem>
        </template>

    </Draggable>

    <footer class="footer"></footer>
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

</script>
<style scoped>
footer {
    height: 2000px;
    background-color: bisque;
}
</style>
