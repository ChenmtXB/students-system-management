<template>
  <ul id="turn-page" style="user-select: none;">
    <!-- <li class="prev-page">上一页</li>
    <li class="active tcdNumber">1</li>
    <li class="tcdNumber">2</li>
    <li class="tcdNumber">3</li>
    <span>...</span>
    <li class="tcdNumber">8</li>
    <li class="tcdNumber">9</li>
    <li class="tcdNumber">10</li>
    <li class="next-page">下一页</li> -->
    <li class="prev-page" :class="{'not-allow': nowPage === 1}" @click="turnPage(-1)">&lt;</li>
    <template v-if="totalPage <= 7">
      <li class="num"
        v-for="i in totalPage"
        :key="i"
        :class="{'cur-page': nowPage === i}"
        @click="turnPage(i)">{{i}}</li>
    </template>
    <template v-else>
      <template v-if="nowPage <= 4">
        <li v-for="i in 6" :key="i"
          @click="turnPage(i)"
          :class="{'cur-page': nowPage === i}"
        >{{i}}</li>
      </template>
      <template v-if="nowPage > 4">
        <li @click="turnPage(1)">1</li>
        <span>...</span>
      </template>
      <template v-if="nowPage > 4 && nowPage <= totalPage - 4">
        <!-- n - 2 => n+(-2) => -2=i => i-3; n + 2 => i -->
        <!-- 判断当前选中样式 => i等于中间数 -->
        <li v-for="i in 5"
          :key="nowPage - 3 + i"
          :class="{'cur-page': i === 3}"
          @click="turnPage(nowPage - 3 + i)"
        >{{ nowPage - 3 + i }}</li>
      </template>
      <template v-if="nowPage <= totalPage - 4">
        <span>...</span>
        <li @click="turnPage(totalPage)">{{totalPage}}</li>
      </template>
      <template v-if="nowPage > totalPage - 4">
        <li v-for="i in 6" :key='i'
          @click="turnPage(totalPage - 6 + i)"
          :class="{'cur-page': totalPage - 6 + i === nowPage}"
        >{{totalPage - 6 + i}}</li>
      </template>
    </template>
    <li class="next-page" :class="{'not-allow': nowPage === totalPage}" @click="turnPage(0)">&gt;</li>
  </ul>
</template>

<script>
import { mapState, mapActions } from 'vuex'
export default {
  data () {
    return {
    }
  },
  methods: {
    ...mapActions(['getStudentList']),
    turnPage (page) {
      this.getStudentList(page)
    }
  },
  computed: {
    // 每页展示10条
    ...mapState({
      totalPage: state => Math.ceil(state.count / 10),
      nowPage: state => state.nowPage
    })
  }
}
</script>

<style lang="scss">
  #turn-page{
    .not-allow{
      cursor: not-allowed;
    }
  }
</style>
