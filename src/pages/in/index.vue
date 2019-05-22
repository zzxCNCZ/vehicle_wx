<template>
  <div >
    <i-row>
      <i-panel title="请输入信息:">
        <i-row>
          <i-col span="20" i-class="col-class">
            <i-input :value="dataForm.qzcsbh" @change="bhChange" type="text" title="强制措施编号" maxlength="-1"  autofocus placeholder="编号" />
          </i-col>
          <i-col span="4" i-class="scan-icon" >
            <image src="/static/images/scan.png" @click="scan"  class="scan-icon"></image>
          </i-col>
        </i-row>
      </i-panel>
      <i-panel >
        <i-input :value="dataForm.vehicleNum" @change="numChange" type="text"  title="车牌号" maxlength="-1"  placeholder="请输入车牌号" />
        <i-input :value="dataForm.remark" @change="remarkChange" type="textarea" title="备注" placeholder="请输入备注(最多100字)" maxlength="200" />
      </i-panel>
      <i-panel title="请上传图片，最多5张:">
        <upload width="200rpx" ref="upload" height="200rpx" max="1" @choosed="choosed" @delete="deleteImg" :srcs="[]"></upload>
        <!--<image v-for="(item,index) in dataForm.filePath" :key="index" :src="item" @click="chooseImage"  class="in-upload"></image>-->
        <!--<image src="/static/images/plus.png" @click="chooseImage"  class="in-upload"></image>-->
      </i-panel>
      <i-button @click="submit" type="info">提交</i-button>
      <i-spin fix v-if="toastShow">上传中...</i-spin>
      <i-toast id="toast" />
    </i-row>
  </div>
</template>

<script>
import Upload from '../../components/upload'
import { $Toast } from '../../../static/iview/dist/base/index'
// Use Vuex
export default {
  data () {
    return {
      toastShow: false,
      dataForm: {
        qzcsbh: '',
        vehicleNum: '',
        remark: '',
        filePath: []
      }
    }
  },
  components: {
    Upload
  },
  onLoad () {
    console.log('loading-in')
    // 解决页面返回后，数据留存问题
    Object.assign(this, this.$options.data())
  },
  computed: {},
  methods: {
    scan () {
      wx.scanCode({
        onlyFromCamera: true,
        success (res) {
          console.log(res)
        }
      })
    },
    bhChange (data) {
      this.dataForm.qzcsbh = data.mp.detail.detail.value
      console.log(this.dataForm.qzcsbh)
    },
    numChange (data) {
      this.dataForm.vehicleNum = data.mp.detail.detail.value
      console.log(this.dataForm.vehicleNum)
    },
    remarkChange (data) {
      this.dataForm.remark = data.mp.detail.detail.value
      console.log(this.dataForm.remark)
    },
    choosed (all) {
      console.log(all)
      this.dataForm.filePath = all.all
    },
    deleteImg (deleteItem) {
      console.log(deleteItem)
    },
    reloadUpload () {
      this.$refs.upload.reloading()
    },
    handleSuccess () {
      $Toast({
        content: '上传成功',
        type: 'success'
      })
    },
    handleError () {
      $Toast({
        content: '上传失败',
        type: 'error'
      })
    },
    submit () {
      console.log(this.value1)
      let that = this
      console.log(that.dataForm.filePath.length)
      console.log(that.dataForm.vehicleNum)
      console.log(that.dataForm.remark)
      that.toastShow = true
      let p = new Promise((resolve, reject) => {
        for (let i = 0; i <= that.dataForm.filePath.length; i++) {
          wx.uploadFile({
            url: 'https://jtglssbx.bjjtgl.gov.cn/sboracle/vehicle/parkin/upload',
            // url: 'http://localhost:8089/sboracle/vehicle/parkin/upload',
            filePath: that.dataForm.filePath[i],
            name: 'image',
            header: {
              'content-type': 'multipart/form-data'
            }, // 设置请求的 header
            formData: {
              qzcsbh: that.dataForm.qzcsbh,
              vehicleNum: that.dataForm.vehicleNum,
              remark: that.dataForm.remark
            }, // HTTP 请求中其他额外的 form data
            success: function (res) {
              console.log(res)
            },
            complete: function (res) {
              if (i === that.dataForm.filePath.length - 1) {
                that.toastShow = false
                that.reloadUpload()
                that.handleSuccess()
                setTimeout(resolve, 2000, 'success')
              }
            }
          })
        }
      })
      p.then(res => {
        console.log(res)
        const url1 = '../index/main'
        wx.redirectTo({
          url: url1
        })
      })
    }
  }
}
</script>

<style scoped>
 .scan-icon {
   width: 50rpx;
   height: 50rpx;
   margin: 20rpx;
  }
.in-upload {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}
</style>
