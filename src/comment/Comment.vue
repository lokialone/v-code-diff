<script setup lang="ts">
import { ref } from 'vue'
import type { CommentType, SplitLineChange } from '../types'
import Content from './Content.vue'

const props = defineProps<{
  line: SplitLineChange
  comments: CommentType[]
}>()
function getShow() {
  const res = props.comments.find((item: CommentType) => {
    const postion = item.postion
    return postion === 0 ? item.markLine === props.line.left.num : item.markLine === props.line.right.num
  })
  return {
    show: !!res,
    postion: res && res.postion,
    info: res,
  }
}
const show = ref(getShow())
</script>

<template>
  <tr v-if="show.show" class="comment-tr">
    <td :class="{ 'comment-space': show.postion === 0 }" />
    <td :class="{ 'comment-space': show.postion === 0 }">
      <div v-show="show.postion === 0" class="comment-content-left">
        <Content :comment="show.info" />
      </div>
    </td>
    <td :class="{ 'comment-space': show.postion === 1 }" />
    <td :class="{ 'comment-space': show.postion === 1 }">
      <div v-show="show.postion === 1" class="comment-content-right">
        <Content :comment="show.info" />
      </div>
    </td>
  </tr>
</template>

<style scoped>
.comment-space {
 background-color: #e5ecf6;
}
.comment-content-td-left {
  background-color: #e5ecf6;
  /* border-right: 1px solid var(--color-border-muted) */
}
.comment-content-left {
  box-sizing: border-box;
}
</style>
