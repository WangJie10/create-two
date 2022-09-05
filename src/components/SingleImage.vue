<template>
  <div v-loading="loading" class="upload-container">
    <el-row>
      <el-col :span="12">
        <el-upload
            :data="dataObj"
            :multiple="false"
            :show-file-list="false"
            :on-success="handleImageSuccess"
            class="image-uploader"
            drag
            accept=".jpg,.jpeg,.png,.gif,.bmp,.JPG,.JPEG,"
            :on-progress="progress"
            :action="$target+'api/upload?bucket=img'"
        >
          <i class="el-icon-upload"/>
          <div class="el-upload__text">
            将图片拖到此处，或<em>点击上传</em>
          </div>
        </el-upload>
      </el-col>
      <el-col :span="12">
        <div class="image-preview">
          <div v-show="imageUrl&&imageUrl.length>1" class="image-preview-wrapper">
            <img :src="$target+imageUrl"/>
            <div class="image-preview-action">
              <i class="el-icon-delete" @click="rmImage"/>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>

export default {
  name: 'SingleImageUpload',
  props: {
    value: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      tempUrl: '',
      dataObj: {token: '', key: ''},
      loading: false
    }
  },
  computed: {
    imageUrl() {
      return this.value
    }
  },
  methods: {
    rmImage() {
      this.emitInput('')
    },
    emitInput(val) {
      this.$emit('input', val)
    },
    handleImageSuccess(response) {
      this.tempUrl = response
      this.emitInput(this.tempUrl)
      this.loading = false
    },
    progress() {
      this.loading = true
    }
  }
}
</script>

<style scoped>
.upload-container {
  width: 100%;
  position: relative;
}

.image-uploader {
  width: 60%;
  float: left;
}

.el-icon-delete {
  font-size: 36px;
}

.image-preview-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
}

.image-preview-wrapper img {
  width: 100%;
  height: 100%;
}

.image-preview-action {
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  color: #fff;
  opacity: 0;
  font-size: 20px;
  background-color: rgba(0, 0, 0, .5);
  transition: opacity .3s;
  cursor: pointer;
  text-align: center;
  line-height: 200px;
}

.image-preview {
  width: 200px;
  height: 200px;
  position: relative;
  border: 1px dashed #d9d9d9;
  float: left;
  margin-left: 50px;
}

.image-preview :hover.image-preview-action {
  opacity: 1;
}

</style>
