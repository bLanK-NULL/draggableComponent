
<template>
    <div class="container_drag" ref="container">
        <!-- 里面会插入<draggable-item> -->
        <div class="item_drag" v-for="item in props.items" :key="item.id">
            <slot :data="item"></slot> 
        </div>
        <SLine></SLine>
    </div>
</template>
<script setup lang="ts">
import DraggableItem from './DraggableItem.vue'
import SLine from './utils/SLine.vue'
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
const emit = defineEmits(['closeLianxian'])
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

        // 1.5 鼠标经过出现四个点的效果
        let spans: Array<HTMLElement> = []
        container.value.addEventListener('mouseover', (e) => {
            const item = whichItem(e)
            if (item) {
                const pos = [{
                    x: item.clientWidth / 2 - 4,
                    y: 0 - 4
                }, {
                    x: item.clientWidth - 4,
                    y: item.clientHeight / 2 - 4
                }, {
                    x: item.clientWidth / 2 - 4,
                    y: item.clientHeight - 4
                }, {
                    x: 0 - 4,
                    y: item.clientHeight / 2 - 4
                }];
                if (spans.length === 0) {//初次生成
                    for (let i = 0; i < 4; i++) {
                        const span = document.createElement('span')
                        const style = span.style;
                        style.position = 'absolute'
                        style.width = '8px'
                        style.height = '8px'
                        style.backgroundColor = '#999'
                        style.borderRadius = '50%'
                        style.overflow = 'hidden'
                        spans.push(span)
                    }
                }
                for (let i = 0; i < 4; i++) {
                    spans[i].style.left = `${pos[i].x}px`
                    spans[i].style.top = `${pos[i].y}px`
                    spans[i].style.display = 'inline'
                    !item.contains(spans[i])  && item.appendChild(spans[i])
                }
            }
        })
        container.value.addEventListener('mouseout',(e)=> {
            const item = whichItem(e)
            if(item) {
                for(let i = 0;i<spans.length;i++){
                    spans[i].style.display = 'none'
                }
            }
        })

        //2. 连线功能
        let waitToLine: Array<HTMLElement | null> = []
        container.value.addEventListener('click', (e) => {

            if (props.lianxianBegin) {
                const item = whichItem(e)

                //存储要连线的两个item
                if (!waitToLine.includes(item) && item && waitToLine.length < 2) {
                    waitToLine.push(item)
                } else {
                    throw new Error('连线功能只支持两个点连线')
                }
                //如果两个盒子都有了，开始连线
                if (waitToLine.length === 2) {
                    console.dir(waitToLine);

                    const pos1 = waitToLine[0]?.getBoundingClientRect()
                    const pos2 = waitToLine[1]?.getBoundingClientRect()
                    console.log(`开始坐标: (${pos1.left + pos1.width / 2},${pos1.top + pos1.height / 2})
                                结束坐标: (${pos2.left + pos2.width / 2},${pos2.top + pos2.height / 2})`)
                    if (pos1 && pos2) {
                        const canvas = document.createElement('canvas')
                        canvas.id = 'myCanvas';
                        const ctx = canvas.getContext('2d')
                        // 绘制直线
                        ctx.strokeStyle = 'black';
                        // ctx.beginPath(); // 开始绘制路径
                        // ctx.moveTo(pos1.left + pos1.width / 2, pos1.top + pos1.height / 2); // 将路径移动到第一个盒子的中心
                        ctx.moveTo(100, 0)
                        ctx.lineTo(pos2.left + pos2.width / 2, pos2.top + pos2.height / 2); // 绘制一条直线连接两个盒子的中心

                        ctx.stroke(); // 绘制路径
                        container.value.appendChild(canvas)
                    }

                    //关闭连线
                    emit('closeLianxian')
                }
            }
        })
    }
})

</script>

<style scoped>
.item_drag {
    position: relative;
    width: fit-content;
    cursor: grab;
    border-radius: 4px;
    margin: 5px;
    box-sizing: border-box;
}

.grabbing {
    cursor: grabbing !important;
}

.item_drag:hover {
    box-shadow: 0 0 5px 2px rgba(0, 0, 0, 0.2);
}
</style>
