<template>
  <div>
    <el-upload
      ref="uploadImage"
      :class="disabled?'avatar-uploader disabled':'avatar-uploader'"
      :action="uploadUrl"
      :headers="headerObj"
      :accept="accept"
      name="file"
      list-type="picture-card"
      :before-upload="beforeUpload"
      :on-success="handleSuccess"
      :on-remove="handleRemove"
      :disabled="disabled"
      :show-file-list="false"
    >
      <img v-if="imageUrl" :src="imageUrl" class="avatar">
      <i v-else class="el-icon-plus avatar-uploader-icon" />
    </el-upload>
  </div>
</template>

<script>
export default {
  name: 'UploadImage',
  props: {
    accept: {
      type: String,
      default: 'image/jpg,image/jpeg,image/png,image/PNG'
    },
    imageUrl: {
      type: String,
      default: ''
    },
    disabled: {
      type: Boolean
    }
  },
  data() {
    return {
      // formData: {

      //   name: ""
      // },
      headerObj: {
        Authorization: 'Basic YW5vbnltb3VzOmFub255bW91cw=='
      },
      dialogVisible: false,
      uploadUrl: '/c/upload/file'
    }
  },
  mounted() {
    this.accept = this.accept || 'image/jpg,image/jpeg,image/png,image/PNG'
  },
  methods: {
    beforeUpload(file) {
      const isLt2M = file.size / 1024 / 1024 < 2
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 2MB!')
      }
      return isLt2M
    },
    handleSuccess(response, file) {
      if (response.code === 100000) {
        // this.imageUrl = URL.createObjectURL(file.raw)
        this.$emit('addFiles', response)
      } else {
        this.$message.warning('上传失败！')
      }
    },
    handleRemove(file) {
      this.$emit('removeFile', file)
    }
  }
}
</script>

<style scoped lang="scss">
    .avatar-uploader{
    width: 150px;
    height: 150px;
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
      position: relative;
    &:hover{
      border-color: #409EFF;
    }
    .avatar-uploader-icon {
      font-size: 28px;
      color: #8c939d;
      width: 150px;
      height: 150px;
      line-height: 150px;
      text-align: center;
    }
    .avatar {
      width: 100%;
      height: 100%;
      display: block;
    }
    .el-upload__tip{
      color:#F56C6C
    }
  }
</style>
