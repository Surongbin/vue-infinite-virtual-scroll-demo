<!--
 * @Description:
 * @Author: cooky
 * @Date: 2020-01-08 09:31:28
 * @LastEditors  : cooky
 * @LastEditTime : 2020-01-09 16:21:22
 -->
<template>
<div v-show="list.length > 0">
  <button @click="handleClick">切换数据</button>
  <RecycleScroller
    class="scroller"
    :items="list"
   :item-size="50"
    key-field="id"
    v-slot="{ item }"
    v-infinite-scroll="loadMore"
    infinite-scroll-disabled="busy"
    infinite-scroll-distance="10"
  >
     {{ item.value }}
  </RecycleScroller>
</div>
</template>

<script>
import infiniteScroll from 'vue-infinite-scroll'

const defaultList = []
for (let i = 0; i < 20; i++) {
  defaultList.push({
    id: i + 1,
    value: 'item' + (i + 1)
  })
}
export default {
  directives: { infiniteScroll },
  data () {
    return {
      list: [],
      busy: false
    }
  },
  methods: {
    handleClick () {
      const otherList = []
      for (let i = 0; i < 20; i++) {
        otherList.push({
          id: 'other' + (i + 1),
          value: 'otheritem' + (i + 1)
        })
      }
      this.list = []
      this.$nextTick(() => {
        this.list = otherList
      })
    },
    loadMore: function () {
      this.busy = true
      const length = this.list.length
      for (var i = length; i < length + 20; i++) {
        this.list.push({
          id: i + 1,
          value: 'item' + (i + 1)
        })
      }
      this.busy = false
    }
  }
}
</script>

<style scoped>
.scroller {
  height: 200px;
}
</style>
