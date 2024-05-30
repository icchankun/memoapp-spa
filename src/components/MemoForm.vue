<script setup>
import { ref, watchEffect } from 'vue'

const props = defineProps({
  showedMemo: Object
})
const emit = defineEmits(['updateMemo', 'deleteMemo'])

const content = ref('')

const updateMemo = (showedMemo, content) => emit('updateMemo', showedMemo, content)
const deleteMemo = (showedMemo) => emit('deleteMemo', showedMemo.id)

watchEffect(() => {
  content.value = props.showedMemo.content
})
</script>

<template>
  <form @submit.prevent id="memo-form">
    <textarea v-model="content" rows="10"></textarea>
    <button @click="updateMemo(showedMemo, content)" :disabled="!content" class="btn">編集</button>
    <button @click="deleteMemo(showedMemo)" class="btn">削除</button>
  </form>
</template>

<style scoped></style>
