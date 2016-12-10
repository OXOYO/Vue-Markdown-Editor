<!--
  Created by OXOYO on 2016/12/10.

  编辑器 组件
-->

<style lang="less">
  .x-editor {
    padding: 0 15px;
    text-align: left;
  }
</style>


<template>
  <div class="x-editor">
    <textarea id="xEditor" style="opacity: 0"></textarea>
  </div>
</template>


<script>
  import SimpleMDE from 'simplemde'

  export default {
    props: {
      config: {
        type: Object,
        default: function () {
          return {
            autoDownloadFontAwesome: false,
            autosave: {
              enabled: true,
              uniqueId: 'xEditor'
            },
            element: '',
            placeholder: '欢迎使用Vue-Markdown-Editor！',
            spellChecker: false,
            toolbar: [
              'bold',
              'italic',
              'strikethrough',
              '|',
              'heading',
              'code',
              'quote',
              '|',
              'unordered-list',
              'ordered-list',
              'clean-block',
              '|',
              'link',
              'image',
              'table',
              '|',
              'horizontal-rule',
              'preview',
              'side-by-side',
              'fullscreen',
              'guide'
            ]
          }
        }
      },
      action: {
        type: String
      },
      resetAction: {
        type: Function
      }
    },
    data () {
      return {
        simplemde: null,
        editor: null,
        contents: null,
        _action: this.action
      }
    },
    watch: {
      action (val) {
        let _t = this

        // 更新_action
        _t._action = val
        console.log('Editor action:', val)
        // 执行相应的操作
        switch (_t.action) {
          case 'getContents':
            _t.getContents()
            break
          case 'clearContents':
            _t.clearContents()
            break
        }
        // 执行回调，更新外层组件的action值
        _t.resetAction && _t.resetAction()
      },
      contents (val) {
        let _t = this

        _t.$emit('on-contents-change', val)
      }
    },
    methods: {
      getContents: function () {
        let _t = this

        _t.contents = _t.simplemde.value()
        console.log('Editor getContents:', _t.contents)
      },
      clearContents: function () {
        let _t = this

        // 清除编辑器内容
        _t.simplemde.clearAutosavedValue()
        _t.simplemde.value('')
        _t.contents = null
      }
    },
    created: function () {
      let _t = this

      setTimeout(function () {
        _t.editor = document.getElementById('xEditor')
        if (!_t.editor) {
          console.log('创建编辑器失败！没有找到挂载的DOM元素。')
        }
        _t.config.element = _t.editor
        // 创建编辑器实例
        _t.simplemde = new SimpleMDE(_t.config)
      }, 0)

      console.log('Editor created:', this.action)
    },
    beforeDestroy: function () {
      let _t = this

      _t.simplemde.toTextArea()
      _t.simplemde = null
    }
  }
</script>
