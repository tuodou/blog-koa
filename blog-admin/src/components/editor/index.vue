<template>
  <div class="editor-box">
    <mavon-editor
      class="editor"
      ref="editor"
      :toolbars="markdownOption"
      placeholder="请开始你的表演..."
      :value="detail"
      @change="editorChange"
      @imgAdd="imageUpload"
    />
  </div>
</template>

<script>
import Vue from 'vue'
import { uploadImageAPi } from '@/api/blogApi'
import mavonEditore from 'mavon-editor'
import 'mavon-editor/dist/css/index.css'
Vue.use(mavonEditore)

export default {
  name: 'Editor',
  props: {
    detail: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      markdownOption: {
        bold: true, // 粗体
        italic: true, // 斜体
        header: true, // 标题
        underline: true, // 下划线
        strikethrough: true, // 中划线
        mark: true, // 标记
        superscript: true, // 上角标
        subscript: true, // 下角标
        quote: true, // 引用
        ol: true, // 有序列表
        ul: true, // 无序列表
        link: true, // 链接
        imagelink: true, // 图片链接
        code: true, // code
        table: true, // 表格
        fullscreen: true, // 全屏编辑
        readmodel: true, // 沉浸式阅读
        htmlcode: true, // 展示html源码
        help: true, // 帮助
        /* 1.3.5 */
        undo: true, // 上一步
        redo: true, // 下一步
        trash: true, // 清空
        save: true, // 保存（触发events中的save事件）
        /* 1.4.2 */
        navigation: true, // 导航目录
        /* 2.1.8 */
        alignleft: true, // 左对齐
        aligncenter: true, // 居中
        alignright: true, // 右对齐
        /* 2.2.1 */
        subfield: true, // 单双栏模式
        preview: true, // 预览
      }
    }
  },
  methods: {
    /**
     * @function 上传图片
     */
    imageUpload (pos, file) {
      let form = new FormData()
      form.append('file', file)
      uploadImageAPi(form).then(res => {
        console.log('upload image', res)
        this.$refs.editor.$img2Url(pos, res.imgUrl)
      })
    },
    /**
     * @function 编辑器内容改变
     * @param {Object} value
     */
    editorChange (value) {
      this.$emit('change', value)
    }
  }
}
</script>

<style lang="less" scoped>
.editor-box {
  max-height: 1200px;
  .editor {
    overflow: auto;
    height: 100%;
  }
}
</style>
