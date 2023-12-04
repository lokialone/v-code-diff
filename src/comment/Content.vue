<script setup lang="ts">
import { reactive } from 'vue'
import type { CommentType } from '../types'

const props = defineProps<{
  comment: CommentType
}>()

const commentInfo = reactive({
  ...props.comment,
  isEdit: false,
})

console.log('--->', props.comment)
</script>

<template>
  <div>
    <div class="comment-line">
      评论行代码 <span>1</span> to <span>8</span>
    </div>
    <div class="user-comment">
      <div class="user-comment-header">
        <img
          class="awatar"
          src="https://static.yximgs.com/udata/pkg/KS-IS-AVATAR-PRODUCTION/60224/FFE74E660B0538CB2E6D6DED32EF327B_compressed_100"
          alt=""
        >
        <span class="name"> 你好</span> <span class="time"> 21小时前</span>
      </div>
      <div class="user-comment-content">
        <input v-if="commentInfo.isEdit" v-model="commentInfo.message" class="ml-40 input">
        <div v-else class="ml-40">
          {{ commentInfo.message }}
        </div>
      </div>
      <div class="user-comment-operation ">
        <span class="text-btn ml-40">回复</span> <span class="text-btn">编辑</span> <span class="text-btn">删除</span>
      </div>
    </div>
    <div class="add-comment">
      <img
        class="awatar"
        src="https://static.yximgs.com/udata/pkg/KS-IS-AVATAR-PRODUCTION/60224/FFE74E660B0538CB2E6D6DED32EF327B_compressed_100"
        alt=""
      >
      <input class="input ml-8" placeholder="添加评论">
    </div>
  </div>
</template>

<style scoped lang="scss">
.comment-line {
  padding: 12px 0;
  font-size: 14px;
}

.awatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
}

.add-comment {
  border-top: 1px solid  rgba(0, 0, 0, .06);
  display: flex;
  align-items: center;
  padding: 12px 0;
}

.user-comment {
  .user-comment-header {
    display: flex;
    align-items: center;
  }
  .user-comment-operation {
    color: rgb(127, 127, 127);
    font-size: 14px;
    margin: 12px;
  }
}

.name {
  font-weight: bold;
  padding: 0 8px;
  font-size: 14px;
}

.time {
  color: #575859;
  font-size: 12px;
}
.ml-8 {
  margin-left: 8px;
}
.ml-40 {
  margin-left: 40px;
}

.input {
  height: 32px;
  border: 1px solid rgba(0, 0, 0, .06);
  padding: 12px;
}
.text-btn {
  padding: 0 12px;
  cursor: pointer;
}
</style>
