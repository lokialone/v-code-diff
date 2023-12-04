<script setup lang="ts">
import type { SplitLineChange, SplitViewerChange } from '../types'
import Comment from '../comment/Comment.vue'
import SplitLine from './SplitLine.vue'

const props = defineProps<{
  diffChange: SplitViewerChange
}>()

function expandHandler({ hideIndex }: SplitLineChange) {
  if (hideIndex === undefined)
    return
  props.diffChange.collector[hideIndex!].lines.forEach((line) => {
    line.hide = false
    line.fold = false
  })
}

const comments = [
  {
    markLine: 4,
    lineRange: [4, 10],
    postion: 0,
    message: 'dddddd',
  },
  {
    markLine: 9,
    lineRange: [6, 10],
    postion: 1,
    message: '你好啊啊啊啊啊啊啊啊aaa ',
  },
]
</script>

<template>
  <table class="file-diff-split diff-table">
    <colgroup>
      <col width="44">
      <col>
      <col width="44">
      <col>
    </colgroup>
    <tbody>
      <template v-for="(item, index) in diffChange?.changes" :key="index">
        <SplitLine :split-line="item" @expand="expandHandler" />
        <Comment :line="item" :comments="comments" />
      </template>
    </tbody>
  </table>
</template>

<style scoped></style>
