<template>
  <div id="app">
    <h3>阵线青年曲库 第{{page}}页</h3>
    <div v-if="items.length" v-for="item in items" :key="item.chant_id" class="box">
      <div>{{item.name}}</div> <audio :src="item.url" controls @play="lyric(item)" />
      <div v-if="play===item.chant_id" class="lyric">{{item.description}}</div>
    </div>
    <div>
      <button class="btn" @click="page--;loadChant()">上一页</button>
      <button class="btn" @click="page++;loadChant()">下一页</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data () {
    return {
      page: 1,
      items: [],
      play: -1
    }
  },
  mounted () {
    this.loadChant()
  },
  methods: {
    loadChant () {
      if (this.page === 0) this.page = 1
      let that = this
      axios.get('https://107754690.dustark.cn/weapp/chants?page=' + this.page)
        .then(function (response) {
          console.log(response)
          if (!response || !response.data || !response.data.chants) {
            that.items = []
          } else {
            that.items = response.data.chants
          }
        })
        .catch(function (response) {
          console.log(response)
        })
    },
    lyric (item) {
      this.play = item.chant_id
    }
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  .box {
    margin: 5px;
    padding: 5px;
    line-height: 20px;
    cursor: pointer;
  }

  .btn {
    padding: 10px 40px 10px 40px;
  }

  .lyric {
    background: #e8eff2;
    font-size: 10px;
    line-height: 14px;
    max-height: 42px;
    overflow-y: auto;
  }
</style>
