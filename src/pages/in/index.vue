<template>
  <div >
    <i-row>
      <i-panel title="请输入信息">
        <i-input :value="dataForm.code" title="强制措施编号" autofocus placeholder="名字" />
        <i-input :value="dataForm.carNumber" type="number" title="车牌号" placeholder="请输入手机号" />
        <i-input :value="dataForm.remark" type="textarea" title="备注" placeholder="请输入详细地址(最多50字)" maxlength="50" />
      </i-panel>
      <i-panel title="请上传图片，最多5张">
        <image v-for="item in 5" src="/static/images/plus.png" @click="chooseImage" data-index='1' class="in-upload"></image>
        <image src="/static/images/plus.png" @click="chooseImage" data-index='1' class="in-upload"></image>
      </i-panel>
      <i-button @click="chooseImage" type="info">提交</i-button>
    </i-row>
  </div>
</template>

<script>
// Use Vuex
export default {
  data () {
    return {
      dataForm: {
        code: '',
        carNumber: '',
        remark: '',
        url1: ''
      }
    }
  },
  computed: {
  },
  methods: {
    chooseImage (e) {
      wx.showActionSheet({
        itemList: ['从相册中选择', '拍照'],
        itemColor: '#f7982a',
        success: function (res) {
        }
      })
    }
  }
}
</script>

<style scoped>
  .in-upload {
    width: 128rpx;
    height: 128rpx;
    margin: 20rpx;
    border-radius: 50%;
  }
</style>
