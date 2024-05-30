<script setup>
defineProps({
  showedMemo: Object,
  memos: Array
})
const emit = defineEmits(['addMemo', 'showMemo'])

const memoFirstLine = (memo) => memo.content.split('\n')[0]
const addMemo = () => emit('addMemo')
const showMemo = (memo, event) => {
  event.preventDefault()
  emit('showMemo', memo)
}
</script>

<template>
  <div id="memo-list">
    <ul id="memos">
      <li v-for="memo in memos" :key="memo.id" class="memo">
        <a v-if="memo.id !== showedMemo.id" href="" @click="showMemo(memo, $event)">{{
          memoFirstLine(memo)
        }}</a>
        <span v-else>{{ memoFirstLine(memo) }}</span>
      </li>
    </ul>
    <a @click="addMemo()" class="btn" id="add-btn">+</a>
  </div>
</template>

<style scoped></style>
