<template>
  <div>
    <van-nav-bar :title="title" left-arrow @click-left="$emit('close')" fixed
      ><template #left><van-icon name="cross" /></template
    ></van-nav-bar>
    <CommentItem :item="comment" class="comment"></CommentItem>
    <van-cell title="全部回复" />
    <CommentItem
      :item="item"
      v-for="item in list"
      :key="item.com_id"
    ></CommentItem>
    <div class="comment1"></div>
    <div class="bottom">
      <van-button
        type="primary"
        block
        round
        plain
        @click="isAddCommentShow = true"
        >评论</van-button
      >
    </div>
    <van-popup position="bottom" v-model="isAddCommentShow"
      ><AddComment
        :target="comment.com_id"
        :art_id="$route.params.article_id"
        v-if="isAddCommentShow"
        @add-comment="
          list.unshift($event);
          isAddCommentShow = false;
          comment.reply_count++;
        "
      ></AddComment
    ></van-popup>
  </div>
</template>

<script>
import AddComment from './AddComment'
import { getComment } from '@/api/comment'
import CommentItem from './CommentItem'
export default {
  props: { comment: { type: Object, default: () => { } } },
  created () { this.getComment() },
  data () {
    return { offset: null, limit: 10, list: [], isAddCommentShow: false }
  },
  methods: {
    // 对评论的接口封装0
    async getComment () {
      try {
        const res = await getComment({ type: 'c', source: this.comment.com_id, offset: this.offset, limit: this.limit })
        console.log(res)
        this.list = res.data.data.results
      } catch (err) { console.log(err) }
    }
  },
  computed: {
    title () {
      const count = this.comment.reply_count
      if (count === 0) { return '暂无回复' } else { return count + '条回复' }
    }
  },
  watch: {},
  filters: {},
  components: { CommentItem, AddComment }
}
</script>

<style scoped lang='less'>
.comment {
  margin-top: 92px;
}
.bottom {
  height: 110px;
  width: 100%;
  position: fixed;
  bottom: 0;
  background-color: green;
  padding: 10px 30px;
  box-sizing: border-box;
}
.comment1 {
  margin-bottom: 110px;
}
</style>
