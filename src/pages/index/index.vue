<template>
  <div @click="clickHandle">

    <!--<div class="userinfo" @click="bindViewTap">-->
      <!--<img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />-->
      <!--<img class="userinfo-avatar" src="/static/images/user.png" background-size="cover" />-->

      <!--<div class="userinfo-nickname">-->
        <!--<card :text="userInfo.nickName"></card>-->
      <!--</div>-->
    <!--</div>-->

    <!--<div class="usermotto">-->
      <!--<div class="user-motto">-->
        <!--<card :text="motto"></card>-->
      <!--</div>-->
    <!--</div>-->
    <i-row>
      <img  class="main-head" src="/static/images/parking.jpg" background-size="cover" />
    </i-row>
    <i-row>
      <i-button type="error" @click="carIn">入场</i-button>
    </i-row>
    <i-row>
      <i-button type="success" @click="carOut" >出场</i-button>
    </i-row>
    <!--<i-card full title="卡片标题" extra="额外内容" thumb="https://i.loli.net/2017/08/21/599a521472424.jpg">-->
      <!--<view slot="content">内容不错</view>-->
      <!--<view slot="footer">尾部内容</view>-->
    <!--</i-card>-->
    <!--<i-button type="success">按钮</i-button>-->

    <!--<form class="form-container">-->
      <!--<input type="text" class="form-control" :value="motto" placeholder="v-model" />-->
      <!--<input type="text" class="form-control" v-model="motto" placeholder="v-model" />-->
      <!--<input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />-->
    <!--</form>-->

    <!--<a href="/pages/counter/main" class="counter">去往Vuex12示例页面</a>-->

    <!--<div class="all">-->
        <!--<div class="left">-->
        <!--</div>-->
        <!--<div class="right">-->
        <!--</div>-->
    <!--</div>-->
  </div>
</template>

<script>
import card from '@/components/card'

export default {
  data () {
    return {
      motto: 'Hello miniprograme',
      userInfo: {
        nickName: 'mpvue',
        avatarUrl: 'http://mpvue.com/assets/logo.png'
      }
    }
  },

  components: {
    card
  },

  methods: {
    carIn () {
      const url = '../in/main'
      mpvue.navigateTo({ url })
    },
    carOut () {
      const url = '../out/main'
      mpvue.navigateTo({ url })
    },
    bindViewTap () {
      const url = '../logs/main'
      if (mpvuePlatform === 'wx') {
        mpvue.switchTab({ url })
      } else {
        mpvue.navigateTo({ url })
      }
    },
    clickHandle (ev) {
      console.log('clickHandle:', ev)
      // throw {message: 'custom test'}
    }
  },

  created () {
    // let app = getApp()
  }
}
</script>

<style scoped>


.main-head {
  width: 100%;
}
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}
.all{
  width:7.5rem;
  height:1rem;
  background-color:blue;
}
.all:after{
  display:block;
  content:'';
  clear:both;
}
.left{
  float:left;
  width:3rem;
  height:1rem;
  background-color:red;
}

.right{
  float:left;
  width:4.5rem;
  height:1rem;
  background-color:green;
}
</style>
