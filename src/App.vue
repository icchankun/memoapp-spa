<script setup>
import { ref } from 'vue'
import MemoForm from './components/MemoForm.vue'
import MemoList from './components/MemoList.vue'

const showedMemo = ref({})
const isEditing = ref(false)

const fetchSavedId = () => {
  const savedId = localStorage.getItem('id')
  return savedId ? JSON.parse(savedId) : 0
}
const fetchSavedMemos = () => {
  const savedMemos = localStorage.getItem('memos')
  return savedMemos ? JSON.parse(savedMemos) : []
}

const id = ref(fetchSavedId())
const memos = ref(fetchSavedMemos())

const showMemo = (memo) => {
  isEditing.value = true
  showedMemo.value = memo
}
const addMemo = () => {
  showMemo({ id: ++id.value, content: '新規メモ' })
  memos.value.push(showedMemo.value)
  localStorage.setItem('memos', JSON.stringify(memos.value))
  localStorage.setItem('id', showedMemo.value.id)
}
const updateMemo = (memo, updatedContent) => {
  isEditing.value = false
  showedMemo.value = {}
  const memoIndex = memos.value.indexOf(memo)
  memos.value.splice(memoIndex, 1, { ...memo, content: updatedContent })
  localStorage.setItem('memos', JSON.stringify(memos.value))
}
const deleteMemo = (showedMemoId) => {
  isEditing.value = false
  memos.value = memos.value.filter((memo) => memo.id !== showedMemoId)
  localStorage.setItem('memos', JSON.stringify(memos.value))
}
</script>

<template>
  <MemoList :showedMemo="showedMemo" :memos="memos" @addMemo="addMemo" @showMemo="showMemo" />
  <MemoForm
    v-show="isEditing"
    :showedMemo="showedMemo"
    @updateMemo="updateMemo"
    @deleteMemo="deleteMemo"
  />
</template>

<style scoped></style>
