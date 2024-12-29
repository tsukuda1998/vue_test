<!--./src/App.vue-->
<script setup lang="ts">
import { reactive, ref } from 'vue'

/** 
 * 配列の要素のインデックスを from → to に移動させて返す (非破壊)
 */
function moveIndex<T>(original: T[], from: number, to: number): T[] {
  const arr = [...original]          // 元の配列をコピー
  const target = arr[from]           // 移動させたい要素
  arr.splice(from, 1)                // もとの場所から取り除き
  arr.splice(to, 0, target)          // 新しい場所に挿入
  return arr
}

// 並べ替え対象のリストを reactive データで保持
const data = reactive({
  items: [
    { name: '太郎' },
    { name: '花子' },
    { name: '健太' },
    { name: '愛' },
  ],
})

// 「ドラッグ開始時に、どの要素を掴んだか」を記録しておくためのref
const dragFromIndex = ref<number | null>(null)

/** ドラッグ開始時：どの index を掴んだかを覚えておく */
function saveFromIndex(index: number) {
  dragFromIndex.value = index
}

/** 
 * ドロップされた時："掴んだ要素"を"ドロップ先の index" の位置に移動 
 */
function moveItem(toIndex: number) {
  // 何もドラッグしていなければ何もしない
  if (dragFromIndex.value === null) return
  // moveIndex を使って配列を並べ替える
  data.items = moveIndex(data.items, dragFromIndex.value, toIndex)
  // 終わったらリセット
  dragFromIndex.value = null
}
</script>

<template>
  <h2>要素の並べ替えサンプル</h2>

  <div v-for="(person, i) in data.items" :key="i" style="margin-bottom: 8px;">
    <!-- ドロップ要素 -->
    <div 
      class="drop-area" 
      @drop="() => moveItem(i)" 
      @dragover.prevent
    >
      {{ i }}番目
    </div>

    <!-- ドラッグ要素 -->
    <span 
      class="drag-target" 
      draggable="true" 
      @dragstart="() => saveFromIndex(i)"
    >
      {{ person.name }}
    </span>
  </div>

  <!-- 一番下にもドロップ領域を用意すると、最後の要素より下に挿入できる -->
  <div 
    class="drop-area" 
    @drop="() => moveItem(data.items.length)" 
    @dragover.prevent
  >
    下に追加
  </div>
</template>

<style scoped>
.drag-target {
  cursor: move;
  background-color: #4caf50;
  color: white;
  padding: 4px 8px;
  margin-left: 8px;
  user-select: none;
  display: inline-block;
}

.drop-area {
  width: 80px;
  height: 24px;
  background-color: gray;
  color: white;
  text-align: center;
  margin-bottom: 4px;
}
</style>

