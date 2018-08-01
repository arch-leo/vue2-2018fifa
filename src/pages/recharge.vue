<template>
  <div class="recharge">
    <div class="back-head">
      <i class="iconfont icon-arrow-left" @click="goback"></i>
      <h3>充值中心</h3>
    </div>
    <div class="recharge-cont">
      <h4>
        <i class="iconfont icon-info"></i>
        <span>友情提示：长按二维码添加好友进行充值</span>
      </h4>
      <p>
        <img src="../assets/default.png" />
        <span>{{userInfo.username}}</span>
        <span>余额：<em class="color-red">{{userInfo.accountbalance}}</em></span>
      </p>
      <div>
        <img src="../assets/charge.jpg" />
        <span>长按二维码添加好友进行充值</span>
      </div>
    </div>
  </div>
</template>

<script>
import {mapMutations} from 'vuex'
import {setItem, delItem} from '@/utils/localstorage'
import {apiUserinfo} from '@/config/api'
export default {
  name: 'my',
  data () {
    return {
      userInfo: {}
    }
  },
  created () {
    this.popLoadToggle(true)
    this.getDatas()
  },
  methods: {
    ...mapMutations(['setUserInfo', 'popLoadToggle', 'popMsgToggle', 'popAffirmToggle']),
    getDatas () {
      apiUserinfo()
        .then(json => {
          let jsonState = json.data.state
          let jsonDesc = json.data.desc
          let jsonData = json.data.data.result[0]
          this.popLoadToggle(false)
          if (jsonState === 0) {
            delItem('userInfo')
            this.setUserInfo(null)
            this.popAffirmToggle({flag: true, title: '登录已过期,请重新登录', path: '/log', redirect: this.$route.fullPath})
          } else if (jsonState === 1) {
            this.userInfo = jsonData
            this.setUserInfo(jsonData)
            setItem('userInfo', jsonData)
          } else {
            this.popMsgToggle(jsonDesc)
          }
        })
        .catch((err) => {
          if (err) {
            // console.log(err)
          }
          this.popLoadToggle(false)
        })
    },
    goback () {
      this.$router.go(-1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.recharge{height:100%;padding-top:.8rem;overflow-y:auto;}
.recharge-cont h4{background:#ffdfbf;height:.55rem;font-size:.24rem;color:#d66900;padding:0 .1rem;}
.recharge-cont h4 span{line-height:.55rem;}
.recharge-cont p{background:#fff;height:1.6rem;padding:.2rem 0;margin-bottom:.6rem;}
.recharge-cont p img{width:1rem;height:1rem;margin:.1rem .3rem;float:left;}
.recharge-cont p span{display:block;line-height:.6rem;}
.recharge-cont div{padding:0 .6rem;text-align:center;}
.recharge-cont div img{display:block;width:100%;}
.recharge-cont div span{display:block;line-height:.8rem;color:#999;}
</style>
