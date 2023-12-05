
<template>
    <div class="container" ref="container">
        <!-- 里面会插入<draggable-item> -->
        <slot></slot>
    </div>
</template>
<script setup lang="ts">
import DraggableItem from './DraggableItem.vue'
import { onMounted, ref } from 'vue'
const container = ref<HTMLElement | null>(null)
let currentItem: HTMLElement | null = null
onMounted(() => {
    if (container.value) {
        let containerX = 0;
        let containerY = 0;
        const mousemoveHandler = (e: MouseEvent) => {
            containerX += e.movementX
            containerY += e.movementY
            container.value && (container.value.style.transform = `translate(${containerX}px, ${containerY}px)`)
        }
        //draggable盒子被点击
        container.value.addEventListener('mousedown', (e: MouseEvent) => {
            if (e.target) {
                console.log('mousedown', (e.target as HTMLElement).parentElement) //是draggable-item
                currentItem = (e.target as HTMLElement).parentElement;
                container.value && (container.value.style.cursor = 'grabbing')
                window.addEventListener('mousemove', mousemoveHandler)
            }
        })
        container.value.addEventListener('mouseup', (e) => {
            // console.log('mouseup...')
            container.value && (container.value.style.cursor = 'grab')
            window.removeEventListener('mousemove', mousemoveHandler)
        })

    }
})

</script>

<style scoped>
.container {
    width: 120px;
    height: 50px;
    display: inline-block;
    position: relative;
    left: 0;
    top: 0;
    transition: none 0.5s ease;
    transition-property: filter, width, height, left, top;
}

.container:hover {
    cursor: grab;
    left: -4px;
    top: -2px;
    width: 131px;
    height: 55px;
    /*box-shadow: 0 0 5px 2px  rgba(0, 0, 0, 0.2);*/
    filter: drop-shadow(0 0 5px 2px rgba(0, 0, 0, 0.2));
}
</style>
