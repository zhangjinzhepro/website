<template>
  <div class="card p-2" style="min-height: 300px" v-loading="loading">
    <div class="fw-bold mb-1">热议</div>
    <article-item class="mb-2" v-for="item in list" :key="item.id" :data="item" type="hot"></article-item>
  </div>
</template>

<script>
import ArticleItem from '@/views/home/components/ArticleList/Item'
export default {
  name: 'Hot',
  components: { ArticleItem },
  data () {
    return {
      loading: true,
      list: []
    }
  },
  created () {
    this.getHotList()
  },
  methods: {
    async getHotList () {
      this.loading = true
      const params = {
        tag: '',
        page: 0,
        sort: 'answer',
        limit: 6
      }
      const { data } = await this.$fetch.get('/public/hot_list', params)
      this.list = data.data.list
      this.loading = false
    }
  }
}
</script>
