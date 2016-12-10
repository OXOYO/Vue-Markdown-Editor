<template>
  <div id="app">
    <!--<img src="./assets/logo.png">-->
    <h3>Vue-Markdown-Editor</h3>
    <div class="row">
      <div class="col-md-offset-3 col-md-6">
        <x-editor :action="action" :resetAction="resetAction" @on-contents-change="onContentsChange"></x-editor>
      </div>
    </div>
    <div class="row row-btns">
      <div class="col-md-offset-3 col-md-6">
        <button type="button" class="btn btn-primary" @click="handleAction('getContents')">获取内容</button>
        <button type="button" class="btn btn-primary" @click="handleAction('clearContents')">清空内容</button>
        <p class="bg-info text-info" style="padding: 10px; margin-top: 10px;">!! 获取到的内容会在控制台打印出来</p>
      </div>
    </div>
  </div>
</template>

<script>
import Editor from './components/Editor'

export default {
  name: 'app',
  components: {
    'x-editor': Editor
  },
  data () {
    return {
      // 操作
      action: null,
      // 内容
      contents: null
    }
  },
  methods: {
    handleAction: function (action) {
      let _t = this

      _t.action = action || null
    },
    resetAction: function () {
      let _t = this

      _t.action = null
    },
    onContentsChange: function (val) {
      console.log('App action:', this.action)
      console.log('App onContentsChange:', val)
      console.log('------------------------------')
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
    margin-top: 30px;
  }
  .row-btns {
    text-align: left;
    margin: 20px 0;
  }
</style>
