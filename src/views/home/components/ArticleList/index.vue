<template>
  <div class="card p-2">
    <div v-loading="loading" style="min-height: 300px" class="position-relative">
    <div class="d-flex align-items-center mb-2">
      <div class="me-auto">
        <router-link class="finger btn btn-sm" tag="span" to="/home/index" >综合</router-link>
        <span class="mx-2 opacity-25">|</span>
        <router-link tag="span" to="/home/ask" class="finger btn btn-sm">提问</router-link>
        <span class="mx-2 opacity-25">|</span>
        <router-link tag="span" to="/home/share" class="finger btn btn-sm">分享</router-link>
        <span class="mx-2 opacity-25">|</span>
        <router-link tag="span" to="/home/advice" class="finger btn btn-sm">建议</router-link>
      </div>
      <div class="fs-7 d-flex justify-content-end">
        <span class="finger" :class="{'link-primary': sort === 'created' }" @click="changeSort('created')">按最新</span>
        <span class="mx-2">|</span>
        <span class="finger" :class="{'link-primary': sort === 'answer'}" @click="changeSort('answer')">按热议</span>
      </div>
    </div>
      <article-item class="mb-2" v-for="item in list" :key="item.id" :data="item" type="list"></article-item>
    </div>
    <div class="mt-2 text-center">
      <button type="button" class="btn btn-outline-primary btn-sm me-2 finger" @click="prev" :disabled="page === 0">上一页</button>
      <span class="me-2 fs-7">当前页: {{page + 1}}</span>
      <button type="button" class="btn btn-outline-primary btn-sm finger" @click="next" :disabled="nextDisable">下一页</button>
    </div>
  </div>
</template>

<script>
import ArticleItem from '@/views/home/components/ArticleList/Item'
import { getStorage } from 'wanado/src/sources/getStorage'
export default {
  name: 'ArticleList',
  components: { ArticleItem },
  data () {
    return {
      loading: true,
      nextDisable: false,
      sort: 'created',
      page: 0,
      list: []
    }
  },
  props: {
    type: {
      type: String,
      default: 'index'
    }
  },
  created () {
    console.log(getStorage('userInfo'))
    this.getList()
  },
  watch: {
    '$route.params' () {
      this.getList()
    }
  },
  methods: {
    changeSort (value) {
      if (this.sort === value) return
      this.sort = value
      this.getList()
    },
    prev () {
      this.page--
      this.getList()
    },
    next () {
      this.page++
      this.getList()
    },
    async getList () {
      this.loading = true
      const params = {
        tag: this.$route.params.tag === 'index' ? '' : this.$route.params.tag,
        page: this.page,
        sort: this.sort,
        limit: 10
      }
      const { data } = await this.$fetch.get('/public/post_list', params)
      this.list = data.data.list
      this.nextDisable = this.list.length < 10
      this.loading = false
    }
  }
}
</script>
