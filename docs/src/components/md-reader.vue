<template>
  <div class="md-reader">
    <vue-markdown :prerender="onPrerender" :postrender="postrenderSpin"></vue-markdown>
  </div>
</template>

<script>
import get from 'lodash/get.js'
import VueMarkdown from 'vue-markdown'

export default {
  name: 'MdReader',
  components: {
    VueMarkdown
  },
  props: {
    source: String,
    postrender: Function
  },
  data () {
    return {
      text: ''
    }
  },
  methods: {
    onPrerender () {
      const txt = this.source || get(this.$slots, 'default[0].text', '')
      return this.removeSpace(txt)
    },
    removeSpace (str) {
      // 递归去掉开头的换行和空格，避免渲染失败
      if (str.startsWith(' ') || str.startsWith('\n')) {
        return this.removeSpace(str.substring(1))
      }
      return str
    },
    postrenderSpin (str) {
      let result = str
      if (this.postrender) {
        result = this.postrender(str)
      }
      result = result.replace(/class="table"/g, 'class="doc-table row"')
      return result
    }
  }
}
</script>

<style lang="scss" scoped>
.md-reader{
  code, mark {
    margin: 0 4px;
    display: inline-block;
    padding: 0px 5px;
    font-size: 12px;
    border-radius: 3px;
    height: 18px;
    line-height: 18px;
  }
  code{
    color: #3890ff;;
    background-color: #e6effb;
  }
  mark{
    background: yellowgreen;
    color: #5e6d82;
  }
  ol{
    padding: 2px 0 2px 16px;
  }
  hr{
    margin: 2px 0;
  }
  a{
    color: #3890ff;
  }
}
</style>
