<template>
    <!-- <div class="circleContainer"> -->
    <!-- 循环生成4个span -->
    <div class="circleContainer" ref="circleContainer">
        <span :class="{ [`dot__${n}`]: true }" v-for="n in 4" :key="n" ref="spansRef"></span>
    </div>
    <!-- </div> -->
</template>

<script setup lang="ts">
import { computed, onMounted, ref, watch } from 'vue';
const circleContainer = ref<HTMLElement | null>(null)

onMounted(() => {
    circleContainer.value?.addEventListener('mousedown', (e: MouseEvent) => {
        console.log(e.target);
        // console.log(e.x, e.y);
        // console.log(e.layerX, e.layerY)

    })
})

</script>

<style scoped>
span {
    position: absolute;
    width: 8px;
    height: 8px;
    background-color: #999;
    border-radius: 50%;
    overflow: hidden;
}

span:hover {
    cursor: pointer !important;
    ;
}

.dot__1 {
    left: 50%;
    top: 0;
    transform: translate(-50%, -50%);
}

.dot__2 {
    right: 0;
    top: 50%;
    transform: translate(50%, -50%);
}

.dot__3 {
    bottom: 0;
    left: 50%;
    transform: translate(-50%, 50%);
}

.dot__4 {
    left: 0;
    top: 50%;
    transform: translate(-50%, -50%)
}
</style>