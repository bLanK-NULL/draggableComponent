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
</script>

<style scoped>
.item {
    /*item 要全包括内容物(不包括margin), 默认上下margin=5px*/
    /*display: inline-block;*/

    width: fit-content;
    border-radius: 4px;
    overflow: hidden;
}


</style>