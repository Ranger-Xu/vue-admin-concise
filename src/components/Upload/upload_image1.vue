<template>
  <div>
    <el-upload
      ref="uploadImage"
      :action="uploadUrl"
      :headers="headerObj"
      :accept="accept"
      name="file"
      :limit="limit"
      :on-exceed="overLimit"
      list-type="picture-card"
      :on-success="handleSuccess"
      :on-remove="handleRemove"
      :disabled="disabled"
      :file-list="fileList"
      :before-remove="beforeRemove"
    >
      <i class="el-icon-plus avatar-uploader-icon" />
    </el-upload>
  </div>
</template>

<script>
export default {
  name: "UploadImage",
  props: {
    accept: {
      type: String,
      default: "image/jpg,image/jpeg,image/png,image/PNG",
    },
    limit: {
      type: Number,
      default: 1,
    },
    disabled: {
      type: Boolean,
    },
    fileList: {
      type: Array,
      default: () => {
        return [];
      },
    },
  },
  data() {
    return {
      headerObj: {
        Authorization: "Basic YW5vbnltb3VzOmFub255bW91cw==",
      },
      dialogVisible: false,
      uploadUrl: "/c/upload/file",
    };
  },
  watch: {
    files: {
      handler(val, oldVal) {
        console.log(val);
        if (this.fileList.length < 1 && val) {
          const data = JSON.parse(val);
          this.fileList.push({
            name: data.fileName,
            id: data.id,
            file: data.url,
          });
        }
      },
      deep: true,
    },
  },
  mounted() {
    this.accept = this.accept || "image/jpg,image/jpeg,image/png,image/PNG";
  },
  methods: {
    handleSuccess(response, file) {
      if (response.code === 100000) {
        this.$emit("addFiles", response,file);
      } else {
        this.$message.warning("上传失败！");
      }
    },
    beforeRemove(file) {
      return this.$confirm(`确定移除 ${file.fileName}？`);
    },
    handleRemove(file, list) {
      this.$emit("removeFile", file, list);
    },
    overLimit() {
      this.$message.warning("超过上传个数限制！");
    },
  },
};
</script>

<style scoped lang="scss">
.avatar-uploader {
  width: 150px;
  height: 150px;
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  &:hover {
    border-color: #409eff;
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
  .el-upload__tip {
    color: #f56c6c;
  }
}
</style>
