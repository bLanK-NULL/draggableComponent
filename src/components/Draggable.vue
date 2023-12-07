
<template>
    <div class="container_drag" ref="container">
        <!-- 里面会插入<draggable-item> -->
        <div class="item_drag" v-for="item in props.items" :key="item.id">
            <slot :data="item"></slot>
        </div>
    </div>
</template>
<script setup lang="ts">
import DraggableItem from './DraggableItem.vue'
import { onMounted, ref, computed } from 'vue'
const container = ref<HTMLElement | null>(null)
let currentItem: HTMLElement | null = null


const props = defineProps({
    items: {
        type: Array,
        default: []
    },
    lianxianBegin: {
        type: Boolean,
        default: false
    }
})
//1. refs 挂载在slot上拿不到 ， 
function whichItem(e: Event): HTMLElement | null {
    let item = e.target as HTMLElement;
    //item存在，item不是container，item没有item_drag类名 => 往上找
    while (item && item !== container.value && !item.classList.contains('item_drag')) {
        item = item.parentElement as HTMLElement
    }
    //如果它不属于任何一个Item返回null，否则返回他的Item
    return item === container.value ? null : item

}
onMounted(() => {
    //weakmap 里面保存currentItem对应的moveOnX和moveOnY
    const weakmap = new WeakMap()
    if (container.value) {
        const mousemoveHandler = (e: MouseEvent) => {
            const move = weakmap.get(currentItem as object)
            if (!move) {
                weakmap.set(currentItem, {
                    moveOnX: 0,
                    moveOnY: 0,
                })
            } else {
                move.moveOnX += e.movementX
                move.moveOnY += e.movementY
            }
            // container.value && (container.value.style.transform = `translate(${moveOnX}px, ${moveOnY}px)`)
            move && currentItem && (currentItem.style.transform = `translate(${move.moveOnX}px, ${move.moveOnY}px)`)
        }
        //draggable盒子被点击
        container.value.addEventListener('mousedown', (e: MouseEvent) => {
            currentItem = whichItem(e)
            //确认点击的是draggable-item或者里面的东西（有效点击）
            if (currentItem) {
                currentItem.classList.add('grabbing')
                window.addEventListener('mousemove', mousemoveHandler)
            }
        }, true)
        container.value.addEventListener('mouseup', (e) => {
            // container.value && (container.value.style.cursor = 'default')
            //删掉grabbing类
            currentItem && currentItem.classList.remove('grabbing')
            window.removeEventListener('mousemove', mousemoveHandler)
        })


        //2. 连线功能
    }
})

</script>

<style scoped>


.item_drag {
    width: fit-content;
    cursor: grab;
}

.grabbing {
    cursor: grabbing !important;
}

.item_drag:hover {
    box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.2);
}
</style>
