<template>
    <h1>draggable component</h1>
    <Anything></Anything>
    <hr>
    <Draggable>
        <DraggableItem>
            <Anything />
        </DraggableItem>
    </Draggable>
</template>
<script setup lang="ts">
import Anything from './components/Anything.vue';
import Draggable from './components/Draggable.vue';
import DraggableItem from './components/DraggableItem.vue';
</script>
<style scoped></style>
