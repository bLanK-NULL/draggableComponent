<template>
    <div class="btn" @click="function() {console.log('内容物被click')}">
        <div>{{ props.data.text }}</div>
    </div>
</template>

<script setup lang="ts">
import { onMounted } from 'vue';

const props = defineProps({
    data: {
        default: []
    }
})

</script>

<style scoped>
/*内容物最好是border-box*/
.btn {
    box-sizing: border-box;
    width: fit-content;
    background-color: aquamarine;
    font-size: 1em;
    text-align: center;
    line-height: 40px;
    padding: 5px;
    user-select: none;

}
</style>