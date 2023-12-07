<template>
    <div ref="item" class="item">
        <slot></slot>
    </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
const item = ref(null)

onMounted(() => {
    item.value.addEventListener('mousedown', (e) => {
        e.stopPropagation()
    }, true)
})
// defineExpose({
//     moveOnX,
//     moveOnY
// })
</script>

<style scoped>
.item {
    /*item 要全包括内容物, 默认上下margin=5px*/
    width: fit-content;
    margin: 5px 0;
}


</style>