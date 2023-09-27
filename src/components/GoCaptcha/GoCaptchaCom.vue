<template>
  <div class="go-captcha-com-box">
    <div class="go-captcha-wrap">
      <GoCaptchaBtn
        class="go-captcha-btn"
        v-model="defaultData.captStatus"
        width="100%"
        height="50px"
        :image-base64="defaultData.captBase64"
        :thumb-base64="defaultData.captThumbBase64"
        @confirm="handleConfirm"
        @refresh="handleRequestCaptCode"
      />
      <!--   弹窗方式   -->
      <!--      <GoCaptchaBtnDialog-->
      <!--              class="go-captcha-btn"-->
      <!--              v-model="captStatus"-->
      <!--              width="100%"-->
      <!--              height="50px"-->
      <!--              :image-base64="captBase64"-->
      <!--              :thumb-base64="captThumbBase64"-->
      <!--              @confirm="handleConfirm"-->
      <!--              @refresh="handleRequestCaptCode"-->
      <!--      />-->

      <!-- tip -->
      <div class="wg-cap-tip">
        <a target="_blank" href="https://github.com/wenlng/go-captcha/releases"
          ><img src="https://img.shields.io/github/v/release/wenlng/go-captcha.svg" alt="version"
        /></a>
        <!-- Github Button -->
        <!-- Place this tag where you want the button to render. -->
        <a
          class="github-button"
          href="https://github.com/wenlng/go-captcha"
          data-size="large"
          data-show-count="true"
          aria-label="Star wenlng/go-captcha on GitHub"
          >Star</a
        >
        <!-- Place this tag where you want the button to render. -->
        <a
          class="github-button"
          href="https://github.com/wenlng/go-captcha/fork"
          data-size="large"
          data-show-count="true"
          aria-label="Fork wenlng/go-captcha on GitHub"
          >Fork</a
        >
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import GoCaptchaBtn from './GoCaptchaBtn.vue'
// import GoCaptchaBtnDialog from './components/GoCaptchaBtnDialog'
import githubBtn from '@/assets/js/github-btn'
import _ from 'lodash-es'

defineOptions({
  name: 'GoCaptchaCom'
})

const defaultData = reactive({
  // 验证码数据
  needCapt: false,
  popoverVisible: true,
  captBase64: '',
  captThumbBase64: '',
  captKey: '',
  captStatus: 'default',
  captExpires: 0,
  captAutoRefreshCount: 0
})

githubBtn()

/**
 * 处理请求验证码
 */
const handleRequestCaptCode = () => {
  defaultData.captBase64 = ''
  defaultData.captThumbBase64 = ''
  defaultData.captKey = ''

  // this.$axios({
  //   method: 'get',
  //   url: '/api/go_captcha_data'
  // }).then((response) => {
  //   const { data = {} } = response
  //   if ((data['code'] || 0) === 0) {
  //     if (_.isEmpty(data)) {
  //       return
  //     }

  //     defaultData.captBase64 = data['image_base64'] || ''
  //     defaultData.captThumbBase64 = data['thumb_base64'] || ''
  //     defaultData.captKey = data['captcha_key'] || ''
  //   } else {
  //     this.$message({
  //       message: `获取人机验证数据失败`,
  //       type: 'warning'
  //     })
  //   }
  // })
}

/**
 * 处理验证码校验请求
 */
const handleConfirm = (dots) => {
  if (_.size(dots) <= 0) {
    this.$message({
      message: `请进行人机验证再操作`,
      type: 'warning'
    })
    return
  }

  let dotArr = []
  _.forEach(dots, (dot) => {
    dotArr.push(dot.x, dot.y)
  })

  // this.$axios({
  //   headers: {
  //     'Content-Type': 'application/x-www-form-urlencoded'
  //   },
  //   method: 'post',
  //   url: '/api/go_captcha_check_data',
  //   data: JSON.stringify({
  //     dots: dotArr.join(','),
  //     key: defaultData.captKey
  //   })
  // }).then((response) => {
  //   const { data = {} } = response

  //   if ((data['code'] || 0) === 0) {
  //     this.$message({
  //       message: `人机验证成功`,
  //       type: 'success'
  //     })
  //     defaultData.captStatus = 'success'
  //     defaultData.captAutoRefreshCount = 0
  //   } else {
  //     this.$message({
  //       message: `人机验证失败`,
  //       type: 'warning'
  //     })
  //     if (defaultData.captAutoRefreshCount > 5) {
  //       defaultData.captAutoRefreshCount = 0
  //       defaultData.captStatus = 'over'
  //       return
  //     }

  //     handleRequestCaptCode()
  //     defaultData.captAutoRefreshCount += 1
  //     defaultData.captStatus = 'error'
  //   }
  // })
}
</script>

<style>
.go-captcha-wrap {
  position: absolute;
  top: 450px;
  left: 50%;
  margin-left: -200px;
  width: 400px;
}

.go-captcha-btn {
  width: 300px !important;
  margin: 0 auto !important;
}

.wg-cap-tip {
  padding: 50px 0 100px;
  font-size: 13px;
  color: #76839b;
  text-align: center;
  line-height: 180%;
  width: 100%;
  max-width: 680px;
}

.wg-cap-tip a {
  display: inline-block;
  margin: 0 5px;
}

.wg-cap-tip a img {
  height: 28px;
}

.wg-cap-tip > span {
  margin: 0 5px;
}
</style>
