<template>
  <div class="login">
    <Row type="flex" justify="center" align="middle" @keydown.enter.native="submitLogin">
      <Col style="width: 368px;">
        <Header />
        <Row>
          <Tabs v-model="tabName">
            <TabPane :label="$t('usernameLogin')" name="username" icon="md-person">
              <Form
                ref="usernameLoginForm"
                :model="form"
                :rules="rules"
                class="form"
                v-if="tabName=='username'"
              >
                <FormItem prop="username">
                  <Input
                    v-model="form.username"
                    prefix="ios-contact"
                    size="large"
                    clearable
                    placeholder="请输入用户名"
                    autocomplete="off"
                  />
                </FormItem>
                <FormItem prop="password">
                  <Input
                    type="password"
                    v-model="form.password"
                    prefix="ios-lock"
                    size="large"
                    clearable
                    placeholder="请输入密码"
                    autocomplete="off"
                  />
                </FormItem>
                <FormItem prop="imgCode">
                  <Row
                    type="flex"
                    justify="space-between"
                    style="align-items: center;overflow: hidden;"
                  >
                    <Input
                      v-model="form.imgCode"
                      size="large"
                      clearable
                      placeholder="请输入图片验证码"
                      :maxlength="10"
                      class="input-verify"
                    />
                    <div style="position:relative">
                      <Spin v-if="loadingCaptcha" fix></Spin>
                      <img
                        :src="captchaImg"
                        @click="getCaptchaImg"
                        alt="加载验证码失败"
                        style="width:100px;cursor:pointer;display:block"
                      />
                    </div>
                  </Row>
                </FormItem>
              </Form>
            </TabPane>
            <TabPane :label="$t('mobileLogin')" name="mobile" icon="ios-phone-portrait">
              <Form
                ref="mobileLoginForm"
                :model="form"
                :rules="rules"
                class="form"
                v-if="tabName=='mobile'"
              >
                <FormItem prop="mobile">
                  <Input
                    v-model="form.mobile"
                    prefix="ios-phone-portrait"
                    size="large"
                    clearable
                    placeholder="请输入手机号"
                  />
                </FormItem>
                <FormItem prop="code" :error="errorCode">
                  <Row type="flex" justify="space-between">
                    <Input
                      v-model="form.code"
                      prefix="ios-mail-outline"
                      size="large"
                      clearable
                      placeholder="请输入短信验证码"
                      :maxlength="6"
                      class="input-verify"
                    />
                    <CountDownButton
                      ref="countDown"
                      @on-click="sendSmsCode"
                      :autoCountDown="false"
                      size="large"
                      :loading="sending"
                      :text="getSms"
                    />
                  </Row>
                </FormItem>
              </Form>
            </TabPane>
          </Tabs>

          <Row type="flex" justify="space-between">
            <Checkbox v-model="saveLogin" size="large">{{ $t('autoLogin') }}</Checkbox>
            <Dropdown trigger="click" @on-click="handleDropDown">
              <a class="forget-pass">{{ $t('forgetPass') }}</a>
              <DropdownMenu slot="list">
                <DropdownItem name="showAccount">体验测试账号</DropdownItem>
                <DropdownItem name="resetByMobile">使用手机号重置密码(付费)</DropdownItem>
                <DropdownItem name="resetByEmail">使用邮箱重置密码(付费)</DropdownItem>
              </DropdownMenu>
            </Dropdown>
          </Row>
          <Row>
            <Button
              class="login-btn"
              type="primary"
              size="large"
              :loading="loading"
              @click="submitLogin"
              long
            >
              <span v-if="!loading">{{ $t('login') }}</span>
              <span v-else>{{ $t('logining') }}</span>
            </Button>
          </Row>
          <Row type="flex" justify="space-between" class="other-login">
            <div class="other-way icons">
              {{ $t('otherLogin') }}
              <div class="other-icon" @click="toGithubLogin">
                <icon scale="1.1" name="brands/github"></icon>
              </div>
              <div class="other-icon" @click="toQQLogin">
                <icon name="brands/qq"></icon>
              </div>
              <div class="other-icon" @click="toWeiboLogin">
                <icon scale="1.2" name="brands/weibo"></icon>
              </div>
              <div class="other-icon" @click="toWeixinLogin">
                <icon scale="1.2" name="brands/weixin"></icon>
              </div>
            </div>
            <router-link to="/regist">
              <a class="forget-pass">{{ $t('regist') }}</a>
            </router-link>
          </Row>
        </Row>
        <Footer />
      </Col>
      <LangSwitch />
    </Row>
  </div>
</template>

<script>
import { login, userInfo, initCaptcha, drawCodeImage } from "@/api/index";
import { validateMobile } from "@/libs/validate";
import Cookies from "js-cookie";
import Header from "@/views/main-components/header";
import Footer from "@/views/main-components/footer";
import LangSwitch from "@/views/main-components/lang-switch";
import CountDownButton from "@/views/my-components/count-down-button";
import util from "@/libs/util.js";
export default {
  components: {
    CountDownButton,
    LangSwitch,
    Header,
    Footer
  },
  data() {
    return {
      captchaId: "",
      captchaImg: "",
      loadingCaptcha: true,
      error: false,
      tabName: "username",
      saveLogin: true,
      getSms: "获取验证码",
      loading: false,
      sending: false,
      errorCode: "",
      form: {
        username: "test或test2 可注册 支持Github、QQ、微博登录",
        password: "123456",
        mobile: "阿里云短信0.045/条 若余额不足联系作者充值",
        code: ""
      },
      rules: {
        username: [
          {
            required: true,
            message: "账号不能为空",
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: "密码不能为空",
            trigger: "blur"
          }
        ],
        imgCode: [
          {
            required: true,
            message: "验证码不能为空",
            trigger: "blur"
          }
        ],
        mobile: [
          {
            required: true,
            message: "手机号不能为空",
            trigger: "blur"
          },
          {
            validator: validateMobile,
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    getCaptchaImg() {
      this.loadingCaptcha = true;
      initCaptcha().then(res => {
        this.loadingCaptcha = false;
        if (res.success) {
          this.captchaId = res.result;
          this.captchaImg = drawCodeImage + this.captchaId;
        }
      });
    },
    sendSmsCode() {
      this.$refs.mobileLoginForm.validate(valid => {
        if (valid) {
          this.$Modal.info({
            title: "抱歉，请获取完整版",
            content: "支付链接: http://xpay.exrick.cn/pay?xboot"
          });
        }
      });
    },
    submitLogin() {
      if (this.tabName == "username") {
        this.$refs.usernameLoginForm.validate(valid => {
          if (valid) {
            this.loading = true;
            login({
              username: this.form.username,
              password: this.form.password,
              code: this.form.imgCode,
              captchaId: this.captchaId,
              saveLogin: this.saveLogin
            }).then(res => {
              if (res.success) {
                this.setStore("accessToken", res.result);
                // 获取用户信息
                userInfo().then(res => {
                  if (res.success) {
                    // 避免超过大小限制
                    delete res.result.permissions;
                    let roles = [];
                    res.result.roles.forEach(e => {
                      roles.push(e.name);
                    });
                    this.setStore("roles", roles);
                    this.setStore("saveLogin", this.saveLogin);
                    if (this.saveLogin) {
                      // 保存7天
                      Cookies.set("userInfo", JSON.stringify(res.result), {
                        expires: 7
                      });
                    } else {
                      Cookies.set("userInfo", JSON.stringify(res.result));
                    }
                    this.setStore("userInfo", res.result);
                    this.$store.commit("setAvatarPath", res.result.avatar);
                    // 加载菜单
                    util.initRouter(this);
                    this.$router.push({
                      name: "home_index"
                    });
                  } else {
                    this.loading = false;
                  }
                });
              } else {
                this.loading = false;
                this.getCaptchaImg();
              }
            });
          }
        });
      } else if (this.tabName == "mobile") {
        this.$refs.mobileLoginForm.validate(valid => {
          if (valid) {
            if (this.form.code == "") {
              this.errorCode = "验证码不能为空";
              return;
            } else {
              this.errorCode = "";
            }
            this.form.saveLogin = this.saveLogin;
            this.$Modal.info({
              title: "抱歉，请获取完整版",
              content: "支付链接: http://xpay.exrick.cn/pay?xboot"
            });
          }
        });
      }
    },
    toGithubLogin() {
      this.$Modal.info({
        title: "抱歉，请获取完整版",
        content: "支付链接: http://xpay.exrick.cn/pay?xboot"
      });
    },
    toQQLogin() {
      this.$Modal.info({
        title: "抱歉，请获取完整版",
        content: "支付链接: http://xpay.exrick.cn/pay?xboot"
      });
    },
    toWeiboLogin() {
      this.$Modal.info({
        title: "抱歉，请获取完整版",
        content: "支付链接: http://xpay.exrick.cn/pay?xboot"
      });
    },
    toWeixinLogin() {
      this.$Message.error("开通微信登录官方收费300/年");
    },
    relatedLogin() {},
    handleDropDown(v) {
      this.$Modal.info({
        title: "抱歉，请获取完整版",
        content: "支付链接: http://xpay.exrick.cn/pay?xboot"
      });
    },
    showAccount() {
      this.$Notice.info({
        title: "体验账号密码",
        desc:
          "账号1：test 密码：123456 <br>账号2：test2 密码：123456 已开放注册！",
        duration: 10
      });
    }
  },
  mounted() {
    this.showAccount();
    this.relatedLogin();
    this.getCaptchaImg();
  }
};
</script>

<style lang="less">
@import "./login.less";
</style>
