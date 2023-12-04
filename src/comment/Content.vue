<script setup lang="ts">
import { reactive, ref, watch } from 'vue'
import Popconfirm from 'ant-design-vue/lib/popconfirm'
import InputNumber from 'ant-design-vue/lib/input-number'

import type { CommentType } from '../types'
import 'ant-design-vue/lib/popconfirm/style'
import 'ant-design-vue/lib/input-number/style'

const props = defineProps<{
  comments: CommentType[]
}>()

const isAddComment = ref(false)
const addNewLineComment = ref(false)
const newLineCommentInfo = reactive({
  markLine: props.comments[0].markLine,
  lineRange: [props.comments[0].markLine, props.comments[0].markLine],
})
// const { updateComments } = inject('comments')

function generateValue() {
  return props.comments.map(item => ({
    ...item,
    comments: item.comments.map(i => ({
      ...i,
      isEdit: false,
    })),
  }))
}
const commentInfo = reactive(generateValue())
const editBeforeValue = ref('')

function edit(comment) {
  editBeforeValue.value = comment.remark
  comment.isEdit = true
}

function cancelEdit(comment) {
  comment.remark = editBeforeValue.value
  comment.isEdit = false
  editBeforeValue.value = ''
  isAddComment.value = false
}

function saveEdit(comment) {
  if (isAddComment.value) {
    //
    isAddComment.value = false
    comment.isEdit = false
  }
  else {
    comment.isEdit = false
    editBeforeValue.value = ''
  }
}

function deleteComment(comment, parentIndex) {
  commentInfo[parentIndex].comments = commentInfo[parentIndex].comments.filter(item => item.id !== comment.id)
}

function replayComment(parentIndex) {
  if (isAddComment.value)
    return
  isAddComment.value = true
  commentInfo[parentIndex].comments.push({
    isEdit: true,
    id: `currnet_${Date.now()}`,
    remark: '',
    time: '',
    isDraft: true,
  })
}

function cancelNewLineComment() {
  addNewLineComment.value = false
  newLineCommentInfo.remark = ''
}

watch(() => props.comments, () => {
  console.log('new props', props.comments)
  commentInfo.value = generateValue()
})
</script>

<template>
  <div>
    <div v-for="(comments, parentIndex) in commentInfo" :key="parentIndex">
      <div class="comment-line">
        评论行代码 <span>{{ comments.lineRange[0] }}</span> to <span>{{ comments.lineRange[1] }}</span>
      </div>
      <div
        v-for="(comment, index) in comments.comments
        " :key="index" class="user-comment"
      >
        <div class="user-comment-header" :class="{ 'ml-40': index !== 0 }">
          <img
            class="awatar"
            src="https://static.yximgs.com/udata/pkg/KS-IS-AVATAR-PRODUCTION/60224/FFE74E660B0538CB2E6D6DED32EF327B_compressed_100"
            alt=""
          >
          <span class="name"> 你好</span> <span class="time"> 21小时前</span>
        </div>
        <div class="user-comment-content" :class="{ 'ml-40': index !== 0 }">
          <div v-if="comment.isEdit" class="mt-12">
            <textarea v-model="comment.remark" class="textarea" />
            <div class="textarea-operation">
              <span class="text-btn color-theme" @click="() => cancelEdit(comment)">取消编辑</span> <span
                class="btn"
                @click="() => saveEdit(comment)"
              >保存</span>
            </div>
          </div>
          <div v-else>
            <div>
              {{ comment.remark }}
            </div>
            <div class="user-comment-operation">
              <span class="text-btn" @click="() => replayComment(parentIndex)">回复</span> <span class="text-btn" @click="() => edit(comment)">编辑</span>
              <Popconfirm
                title="确认删除当前评论" ok-text="确认" cancel-text="取消"
                @confirm="() => deleteComment(comment, parentIndex)"
              >
                <span class="text-btn">删除</span>
              </Popconfirm>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="add-comment">
      <img
        class="awatar mt-12"
        src="https://static.yximgs.com/udata/pkg/KS-IS-AVATAR-PRODUCTION/60224/FFE74E660B0538CB2E6D6DED32EF327B_compressed_100"
        alt=""
      >
      <input v-if="!addNewLineComment" class="input ml-8" placeholder="添加评论" @focus="addNewLineComment = true">
      <div v-else>
        <div> 评论行代码 <InputNumber v-model="newLineCommentInfo.lineRange[0]" /> to <span>{{ newLineCommentInfo.lineRange[1] }}</span></div>
        <textarea class="textarea" />
        <div class="textarea-operation">
          <span class="text-btn color-theme" @click="cancelNewLineComment">取消编辑</span> <span class="btn" @click="() => saveEdit(newLineCommentInfo)">保存</span>
        </div>
      </div>
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
  border-top: 1px solid rgba(0, 0, 0, .06);
  display: flex;
  align-items: start;
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

  .user-comment-content {
    margin-left: 40px;
  }
}

.name {
  font-weight: bold;
  padding: 0 8px;
  font-size: 14px;
}
.textarea-operation {
  display: flex;
  margin-left: 260px;
  margin-top: 12px;
  align-items: center;
  // justify-content: flex-end;
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

.mt-12 {
  margin-top: 12px;
}

.input {
  height: 32px;
  border: 1px solid rgba(0, 0, 0, .06);
  padding: 12px;
}

.text-btn {
  padding: 0 12px;
  cursor: pointer;
  font-size: 14px;
}

.textarea {
  height: 100px;
  min-width: 400px;
  border: 1px solid rgba(0, 0, 0, .06);
  padding: 12px;
}

.color-theme {
  color: #1890ff;
}

.btn {
  background-color: #1890ff;
  display: inline-block;
  color: white;
  font-size: 14px;
  padding: 6px 12px;
  border-radius: 4px;
}
</style>
