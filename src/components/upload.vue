<template>
  <div class="j-pic-upload">
    <img @click="previewImg(index)" v-for="(src,index) in urls" :key="src" :src="src"
         :style="{'width':width || '120rpx','height':height || '120rpx'}" class="img">
    <div class="j-upload-btn" @click="uploadImg(1)" v-if="imageFirstShow"
         :style="{'width':width || '120rpx','height':height || '120rpx'}">
      <span class="j-upload-image">强制凭据</span>
    </div>
    <div class="j-upload-btn" @click="uploadImg(2)" v-if="imageSecondShow"
         :style="{'width':width || '120rpx','height':height || '120rpx'}">
      <span class="j-upload-image">车辆照</span>
    </div>
    <div class="j-upload-btn" @click="uploadImg(3)" v-if="imageThirdShow"
         :style="{'width':width || '120rpx','height':height || '120rpx'}">
      <span class="j-upload-image">交接凭证</span>
    </div>
    <div class="j-upload-btn" @click="uploadImg()" v-if="!imageFirstShow&&!imageSecondShow&&!imageThirdShow&&urls.length<5" :style="{'width':width || '120rpx','height':height || '120rpx'}">
      <span class="j-upload-add">+</span>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['width', 'height', 'max', 'srcs'],
    data () {
      return {
        imageFirstShow: true,
        imageSecondShow: true,
        imageThirdShow: true,
        urls: []
      }
    },
    onLoad () {
      console.log('loading-upload')
      this.imageFirstShow = true
      this.imageSecondShow = true
      this.imageThirdShow = true
      this.urls = this.srcs || []
    },
    mounted () {
      this.imageFirstShow = true
      this.imageSecondShow = true
      this.imageThirdShow = true
      this.urls = this.srcs || []
    },
    methods: {
      reloading () {
        console.log('reloadUpload')
        this.urls = []
      },
      uploadImg (count) {
        let that = this
        wx.chooseImage({
          count: that.max || 3,
          sizeType: ['original', 'compressed'],
          sourceType: ['album', 'camera'],
          success: function (res) {
            res.tempFilePaths.forEach(v => {
              that.urls.push(v)
            })
            that.$emit('choosed', { all: that.urls, currentUpload: res.tempFilePaths })
            if (count === 1) {
              that.imageFirstShow = false
            }
            if (count === 2) {
              that.imageSecondShow = false
            }
            if (count === 3) {
              that.imageThirdShow = false
            }
          }
        })
      },
      previewImg (index) {
        let that = this
        wx.showActionSheet({
          itemList: ['预览', '删除'],
          success: function (res) {
            if (res.tapIndex === 0) {
              wx.previewImage({
                current: that.urls[index],
                urls: that.urls
              })
            } else {
              that.urls.splice(index, 1)
              that.$emit('delete', that.urls)
            }
          }
        })
      }
    }
  }
</script>

<style scoped>
  .in-upload {
    width: 128 rpx;
    height: 128 rpx;
    margin: 20 rpx;
    border-radius: 50%;
  }

  .j-pic-upload {
    padding: 10 rpx;
    display: flex;
    flex-direction: row;
    align-items: center;
    flex-wrap: wrap;
  }

  .j-upload-btn {
    border: 1px dashed #ddd;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin-right: 20 rpx;
  }

  .j-upload-add {
    font-size: 80 rpx;
    font-weight: 500;
    color: #C9C9C9;
  }

  .j-upload-image {
    font-size: 20 rpx;
    font-weight: 500;
    color: #C9C9C9;
  }

  .img {
    margin: 10 rpx 20 rpx 10 rpx 0;
  }
</style>
