<!--components/MySample.vue-->
<template>
  <h1>簡単なアプリ</h1>
  <div v-if="showMessage">
    メッセージが ON　です!
  </div>
  <input 
    type="text"
    :value="textInput"
    @input="onInput"
    @compositionstart="isComposing = true"
    @compositionend="onCompositionEnd"
  />
  <p>入力した内容: {{ textInput }}</p>
  <button @click="toggleMessage">メッセージの表示切替</button>  
</template>


<script setup>
import { ref, computed, watch } from 'vue'

const showMessage = ref(true)
const textInput = ref('')
const isComposing = ref(false)

function onInput(e) {
    textInput.value = e.target.value
}

function onCompositionEnd(e) {
  isComposing.value = false
  textInput.value = e.target.value
}
function toggleMessage() {
  showMessage.value = !showMessage.value
}
</script>




