<script setup lang="ts">
import { computed, inject } from 'vue'
import type { CommentType, SplitLineChange } from '../types'
import Content from './Content.vue'

const props = defineProps<{
  line: SplitLineChange
}>()

const { comments } = inject('comments')
const show = computed(() => {
  const res = comments.value.filter((item: CommentType) => {
    const position = item.position
    return position === 0 ? item.markLine === props.line.left.num : item.markLine === props.line.right.num
  })
  return {
    show: !!res.length,
    position: res.length && res[0].position,
    left: res.filter(item => item.position === 0),
    right: res.filter(item => item.position === 1),
  }
})
</script>

<template>
  <tr v-if="show.show" class="comment-tr">
    <td :class="{ 'comment-space': show.left.length }" />
    <td :class="{ 'comment-space': show.left.length }">
      <div v-if="show.left.length" class="comment-content-left">
        <Content :comments="show.left" />
      </div>
      <div v-else />
    </td>
    <td class="border-left" :class="{ 'comment-space': show.right.length }" />
    <td :class="{ 'comment-space': show.right.length }">
      <div v-if="show.right.length" class="comment-content-right">
        <Content :comments="show.right" />
      </div>
      <div v-else />
    </td>
  </tr>
</template>

<style scoped>
.comment-space {
 background-color: #e5ecf6;
 vertical-align: baseline;
}
.comment-content-td-left {
  background-color: #e5ecf6;
}
.comment-content-left {
  box-sizing: border-box;
}
.border-left {
   border-left: 1px solid var(--color-border-muted)
}
</style>
