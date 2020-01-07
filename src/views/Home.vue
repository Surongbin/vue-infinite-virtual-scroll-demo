<template>
<div v-show="list.length > 0">
  <button @click="handleClick">点击</button>
{{list.length}}
  <RecycleScroller
    class="scroller"
    :items="list"
    :item-size="32"
    key-field="id"
    v-slot="{ item }"
    v-infinite-scroll="loadMore"
    nfinite-scroll-disabled="busy"
    infinite-scroll-distance="10"
  >
    <div class="user">
      {{ item.value }}
    </div>
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
  // props: {
  //   list: Array,
  // },
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
      setTimeout(() => {
        this.list = otherList
      }, 200)
    },
    loadMore: function () {
      console.log('loadMore')
      this.busy = true
      const length = this.list.length
      setTimeout(() => {
        for (var i = length; i < length + 20; i++) {
          this.list.push({
            id: i + 1,
            value: 'item' + (i + 1)
          })
        }
        this.busy = false
      }, 1000)
    }
  }
}
</script>

<style scoped>
.scroller {
  height: 200px;
}

.user {
  height: 32%;
  padding: 0 12px;
  display: flex;
  align-items: center;
}
</style>
