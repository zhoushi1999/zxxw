<template>
  <van-cell>
    <van-image
      slot="icon"
      round
      width="30"
      height="30"
      style="margin-right: 10px"
      :src="item.aut_photo"
    />
    <span style="color: #466b9d" slot="title">hello</span>
    <div slot="label">
      <p style="color: #363636">{{ item.content }}</p>
      <p>
        <span style="margin-right: 10px">{{ item.pubdate | dateformat }}</span>
        <van-button size="mini" type="default" @click="$emit('replsy-show',item)">回复</van-button>
      </p>
    </div>
    <van-loading v-if="isLoading" />
    <van-icon
      v-else
      @click="onClick"
      slot="right-icon"
      :name="item.is_liking ? 'like' : 'like-o'"
      :color="item.is_liking ? 'red' : ''"
      >{{ item.like_count > 0 ? item.like_count : "赞" }}</van-icon
    >
  </van-cell>
</template>

<script>
import { addLike, delLike } from '@/api/comment'
export default {
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  created () { },
  data () {
    return { isLoading: false }
  },
  methods: {
    async onClick () {
      // 点击切换懒加载
      this.isLoading = true
      if (this.item.is_liking) {
        // 取消点赞
        try {
          await delLike(this.item.com_id)
          this.item.is_liking = false
          this.item.like_count--
        } catch (err) {
          console.log(err)
        }
      } else {
        // 点赞
        try {
          await addLike(this.item.com_id)
          this.item.like_count++
          this.item.is_liking = true
        } catch (err) {
          console.log(err)
        }
      }
      this.isLoading = false
    }
  },
  computed: {},
  watch: {},
  filters: {},
  components: {}
}
</script>

<style scoped lang='less'>
</style>
