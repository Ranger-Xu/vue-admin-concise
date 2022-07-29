<template>
  <el-upload
    ref="upload"
    class="upload-demo"
    :action="uploadUrl"
    :headers="headerObj"
    name="file"
    :disabled="disabled"
    :limit="limit"
    :drag="drag"
    :accept="accept"
    :on-exceed="overLimit"
    :on-preview="preview"
    :before-upload="handleUpload"
    :on-success="handleSuccess"
    :on-remove="handleRemove"
    :before-remove="beforeRemove"
    :file-list="fileList"
  >
    <i v-if="drag" class="el-icon-upload" />
    <div v-if="drag" class="el-upload__text">单击此处选择上传文件</div>
    <el-button v-if="!drag" size="mini" type="primary">点击上传</el-button>
    <div slot="tip" :class="position=='bottom'?'el-upload__tip__bottom':'el-upload__tip'">{{ tip }}</div>
  </el-upload>
</template>
<script>
import { defaults } from 'codemirror'
export default {
  name: 'UploadPdf',
  props: {
    accept: {
      type: String,
      default: 'pdf,doc,docx'
    },
    tip: {
      type: String,
      default: ''
    },
    position: {
      type: String,
      default: 'bottom'
    },
    limit: {
      type: Number,
      default: 1
    },
    disabled: {
      type: Boolean
    },
    drag: {
      type: Boolean
    },
    files: {
      type: String
    },
    uploadUrl: {
      type: String,
      default: '/c/upload/file'
    }
  },
  data() {
    return {
      headerObj: {
        Authorization: 'Basic YW5vbnltb3VzOmFub255bW91cw=='
      },
      fileList: []
    }
  },
  methods: {
    handleUpload(file) {
      const type = file.type.split('/')[1]
      if (this.accept.indexOf(type) === -1) {
        this.$message.error(`请上传${this.accept}格式文件`)
        return false
      }
    },
    handleSuccess(response, file) {
      if (response.code === 100000) {
        this.$emit('addFiles', response)
      } else {
        this.$message.warning('上传失败！')
      }
    },
    handleRemove(file) {
      this.$emit('removeFile', file.response)
    },
    beforeRemove(file) {
      return this.$confirm(`确定移除 ${file.name}？`)
    },
    preview(file) {
      window.open(file.response.data.url)
    },
    overLimit() {
      this.$message.warning('超过上传个数限制！')
    }
  }
}
</script>
<style scoped>
  .el-upload__tip {
    display: inline-block !important;
    margin-left: 10px;
    vertical-align: middle;
    margin-top: 0;
  }

  .el-upload__tip__bottom {
    align-self: flex-start !important;
    margin-top: 10px;
    color: #999;
  }

  .el-upload__tip__bottom span {
    color: red !important;
  }
</style>
