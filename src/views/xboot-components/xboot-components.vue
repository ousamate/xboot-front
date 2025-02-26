<template>
  <div>
    <Card>
      <Layout>
        <Sider hide-trigger style="background: #fff;max-width: 205px;flex: 0 0 205px;">
          <Menu
            active-name="1-0"
            theme="light"
            width="auto"
            :open-names="['1','2']"
            @on-select="currName=$event"
          >
            <Submenu name="1">
              <template slot="title">
                <Icon type="md-ionic"/>XBoot通用组件
              </template>
              <MenuItem name="1-0">全局Loading加载动画</MenuItem>
              <MenuItem name="0-0"><Badge dot :offset="[5,-3]">树表格组件</Badge></MenuItem>
              <MenuItem name="1-1">倒计时按钮</MenuItem>
              <MenuItem name="1-2">图标选择输入框</MenuItem>
              <MenuItem name="1-3">部门级联选择</MenuItem>
              <MenuItem name="1-4">部门树选择</MenuItem>
              <MenuItem name="1-5">用户抽屉选择</MenuItem>
              <MenuItem name="1-6">图片上传输入框</MenuItem>
              <MenuItem name="1-7">图片上传缩略图</MenuItem>
              <MenuItem name="1-8">身份验证全屏弹框</MenuItem>
            </Submenu>
            <Submenu name="2">
              <template slot="title">
                <Icon type="md-git-compare"/>工作流组件(付费)
              </template>
              <MenuItem name="2-1">工作流程选择发起</MenuItem>
              <MenuItem name="2-2">通过流程key直接发起</MenuItem>
              <MenuItem name="2-3">取消撤回申请</MenuItem>
            </Submenu>
          </Menu>
        </Sider>
        <Content :style="{padding: '0 24px 24px 24px', minHeight: '280px', background: '#fff'}">
          <div v-show="currName=='0-0'">
            <tree-table/>
          </div>
          <div v-show="currName=='1-0'">
            <Alert type="warning" show-icon>说明：大部分组件为包含真实数据接口的简单封装，方便大家的直接复用！</Alert>
            <Divider class="blue" orientation="left">全局Loading加载动画</Divider>
            <Button @click="showLoading">显示右上角加载动画</Button>
            <Button @click="closeLoading" style="margin-left:5px">关闭右上角加载动画</Button>
            <h3 class="article">使用方式</h3>修改全局Vuex即可，开启：
            <code>this.$store.commit("setLoading", true)</code> 关闭：
            <code>this.$store.commit("setLoading", false)</code>
            <Divider class="blue" orientation="left">iView官方 LoadingBar加载进度条</Divider>
            <Button @click="start">开始加载</Button>
            <Button @click="finish" style="margin-left:5px">结束加载</Button>
            <Button @click="error" style="margin-left:5px">错误</Button>
            <h3 class="article">说明</h3>如果你觉得上方XBoot提供的动画不明显，你还可以使用iView官方自带的顶部
            <a
              href="https://www.iviewui.com/components/loading-bar"
              target="_blank"
            >LoadingBar组件</a>
          </div>
          <div v-show="currName=='1-1'">
            <Divider class="blue" orientation="left">倒计时按钮</Divider>
            <count-down-button countTime="10"/>
            <h3 class="article">提示</h3>你可以将
            <code>autoCountDown</code> 属性设置为
            <code>false</code>，即可手动调用开始倒计时方法
            <code>startCountDown()</code>，用于验证手机号或发送短信成功后开始倒计时等场景。
            <h3 class="article">props</h3>
            <Table :columns="props" :data="data20" border size="small" width="1000"></Table>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data22" border size="small" width="1000"></Table>
            <h3 class="article">methods</h3>
            <Table :columns="methods" :data="data21" border size="small" width="1000"></Table>
          </div>
          <div v-show="currName=='1-2'">
            <Divider class="blue" orientation="left">图标选择输入框</Divider>
            <icon-choose v-model="icon" style="width:400px"></icon-choose>
            <h3 class="article">基础用法</h3>基本用法，使用
            <code>v-model</code> 实现数据的双向绑定。
            <h3 class="article">props</h3>
            <Table :columns="props" :data="data2" border size="small" width="1000"></Table>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data1" border size="small" width="1000"></Table>
          </div>
          <div v-show="currName=='1-3'">
            <Divider class="blue" orientation="left">部门级联选择</Divider>
            <department-choose style="width:300px" @on-change="handleSelectDep" ref="dep"></department-choose>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data3" border size="small" width="1000"></Table>
            <h3 class="article">methods</h3>
            <Table :columns="methods" :data="data4" border size="small" width="1000"></Table>
          </div>
          <div v-show="currName=='1-4'">
            <Divider class="blue" orientation="left">部门树选择</Divider>
            <department-tree-choose
              multiple
              style="width:400px"
              @on-change="handleSelectDepTree"
              ref="depTree"
            ></department-tree-choose>
            <div style="margin-top:10px;">{{selectDeps}}</div>
            <h3 class="article">props</h3>
            <Table :columns="props" :data="data5" border size="small" width="1000"></Table>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data6" border size="small" width="1000"></Table>
            <h3 class="article">methods</h3>
            <Table :columns="methods" :data="data7" border size="small" width="1000"></Table>
          </div>
          <div v-show="currName=='1-5'">
            <Divider class="blue" orientation="left">用户抽屉选择</Divider>
            <user-choose text="点我选择用户" @on-change="handleSelectUser" ref="user"></user-choose>
            <div style="margin-top:10px;">{{selectUsers}}</div>
            <h3 class="article">props</h3>
            <Table :columns="props" :data="data8" border size="small" width="1000"></Table>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data9" border size="small" width="1000"></Table>
            <h3 class="article">methods</h3>
            <Table :columns="methods" :data="data10" border size="small" width="1000"></Table>
          </div>
          <div v-show="currName=='1-6'">
            <Divider class="blue" orientation="left">图片上传文本框</Divider>
            <upload-pic-input v-model="picUrl" style="width:400px" ref="upload"></upload-pic-input>
            <h3 class="article">基础用法</h3>基本用法，使用
            <code>v-model</code> 实现数据的双向绑定。
            <h3 class="article">props</h3>
            <Table :columns="props" :data="data11" border size="small" width="1000"></Table>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data12" border size="small" width="1000"></Table>
          </div>
          <div v-show="currName=='1-7'">
            <Divider class="blue" orientation="left">图片上传缩略图</Divider>
            <upload-pic-thumb @on-change="picUrls=$event" ref="uploadThumb"></upload-pic-thumb>
            {{picUrls}}
            <h3 class="article">props</h3>
            <Table :columns="props" :data="data23" border size="small" width="1000"></Table>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data24" border size="small" width="1000"></Table>
            <h3 class="article">methods</h3>
            <Table :columns="methods" :data="data25" border size="small" width="1000"></Table>
          </div>
          <div v-show="currName=='1-8'">
            <Divider class="blue" orientation="left">身份验证全屏弹框</Divider>
            <Button @click="showCheckPass">开始验证</Button>
            <h3 class="article">events</h3>
            <Table :columns="events" :data="data26" border size="small" width="1000"></Table>
            <h3 class="article">methods</h3>
            <Table :columns="methods" :data="data27" border size="small" width="1000"></Table>
          </div>

          <div v-show="currName=='2-1'||currName=='2-2'||currName=='2-3'">
            <div class="sorry">
              <img src="@/assets/sorry.png">
              <span class="text">抱歉，请获取完整版</span>
              <Button
                to="http://xpay.exrick.cn/pay?xboot"
                target="_blank"
                type="error"
                icon="md-paper-plane"
              >立即获取</Button>
            </div>
          </div>
        </Content>
      </Layout>
    </Card>

    <check-password ref="checkPass" @on-success="checkSuccess"/>
    
  </div>
</template>

<script>
import TreeTable from "./tree-table";
import iconChoose from "@/views/my-components/icon-choose";
import countDownButton from "@/views/my-components/count-down-button";
import departmentChoose from "@/views/my-components/xboot/department-choose";
import departmentTreeChoose from "@/views/my-components/xboot/department-tree-choose";
import userChoose from "@/views/my-components/xboot/user-choose";
import uploadPicInput from "@/views/my-components/xboot/upload-pic-input";
import uploadPicThumb from "@/views/my-components/xboot/upload-pic-thumb";
import checkPassword from "@/views/my-components/xboot/check-password";
export default {
  name: "xboot-components",
  components: {
    TreeTable,
    iconChoose,
    countDownButton,
    departmentChoose,
    userChoose,
    departmentTreeChoose,
    uploadPicInput,
    uploadPicThumb,
    checkPassword
  },
  data() {
    return {
      currName: "1-0",
      processModalVisible: false,
      icon: "",
      selectDeps: [],
      selectUsers: [],
      picUrl: "",
      picUrls: [],
      checkPass: false,
      processLoading: false,
      events: [
        {
          title: "事件名",
          key: "name",
          width: 150
        },
        {
          title: "说明",
          key: "type",
          width: 300
        },
        {
          title: "返回值",
          key: "value"
        }
      ],
      props: [
        {
          title: "属性",
          key: "name",
          width: 130
        },
        {
          title: "说明",
          key: "desc"
        },
        {
          title: "类型",
          key: "type",
          width: 130
        },
        {
          title: "默认值",
          key: "value"
        }
      ],
      methods: [
        {
          title: "方法名",
          key: "name",
          width: 150
        },
        {
          title: "说明",
          key: "type",
          width: 300
        },
        {
          title: "参数",
          key: "value"
        }
      ],
      data1: [
        {
          name: "on-change",
          type: "返回用户选择的图标名或用户输入的图标文本",
          value: "value（输入框文本值）"
        }
      ],
      data2: [
        {
          name: "value",
          desc: "绑定的值，可使用 v-model 双向绑定",
          type: "String",
          value: "空"
        },
        {
          name: "size",
          desc: "输入框尺寸，可选值为large、small、default或者不设置",
          type: "String",
          value: "-"
        },
        {
          name: "placeholder",
          desc: "占位文本",
          type: "String",
          value: "输入图标名或选择图标"
        },
        {
          name: "disabled",
          desc: "设置输入框和选择按钮为禁用状态",
          type: "Boolean",
          value: "false"
        },
        {
          name: "readonly",
          desc: "设置输入框为只读",
          type: "Boolean",
          value: "false"
        },
        {
          name: "maxlength",
          desc: "设置输入框最大输入长度",
          type: "Number",
          value: "-"
        },
        {
          name: "icon",
          desc: "设置上传按钮图标",
          type: "String",
          value: "md-ionic"
        }
      ],
      data3: [
        {
          name: "on-change",
          type: "返回点击部门ID",
          value: "value（点击部门ID）"
        }
      ],
      data4: [
        {
          name: "clearSelect",
          type: "清空已选数据",
          value: "无"
        }
      ],
      data5: [
        {
          name: "placeholder",
          desc: "提示文字",
          type: "String",
          value: "点击选择部门"
        },
        {
          name: "multiple",
          desc: "是否选开启多选，默认false不开启",
          type: "Boolean",
          value: "false"
        }
      ],
      data6: [
        {
          name: "on-change",
          type: "返回选择部门id数组",
          value: '选择部门id数组Array，仅包含部门id，例如 ["1","2","3"]'
        }
      ],
      data7: [
        {
          name: "setData",
          type: "设置已选部门数据（回显使用）",
          value:
            "第一个参数为部门id数组（Array），第二个参数为部门标题（String）"
        }
      ],
      data8: [
        {
          name: "text",
          desc: "选择用户按钮文字",
          type: "String",
          value: "选择用户"
        },
        {
          name: "icon",
          desc: "选择用户按钮图标",
          type: "String",
          value: "md-person-add"
        },
        {
          name: "all",
          desc: "是否选择所有用户",
          type: "Boolean",
          value: "false"
        }
      ],
      data9: [
        {
          name: "on-change",
          type: "返回选择用户数组",
          value:
            '选择用户数组Array，包含用户id和username，例如 [{"id":"1","username":"name"}]'
        }
      ],
      data10: [
        {
          name: "setData",
          type: "设置已选用户数据（回显使用）",
          value:
            '用户数组，需包含用户id和username，例如 [{"id":"1","username":"name"}]'
        }
      ],
      data11: [
        {
          name: "value",
          desc: "绑定的值，可使用 v-model 双向绑定",
          type: "String",
          value: "空"
        },
        {
          name: "size",
          desc: "输入框尺寸，可选值为large、small、default或者不设置",
          type: "String",
          value: "-"
        },
        {
          name: "placeholder",
          desc: "占位文本",
          type: "String",
          value: "可输入图片链接"
        },
        {
          name: "disabled",
          desc: "设置输入框和上传按钮为禁用状态",
          type: "Boolean",
          value: "false"
        },
        {
          name: "readonly",
          desc: "设置输入框为只读",
          type: "Boolean",
          value: "false"
        },
        {
          name: "maxlength",
          desc: "设置输入框最大输入长度",
          type: "Number",
          value: "-"
        },
        {
          name: "icon",
          desc: "设置上传按钮图标",
          type: "String",
          value: "ios-cloud-upload-outline"
        }
      ],
      data12: [
        {
          name: "on-change",
          type: "返回完整上传图片路径或用户输入的链接",
          value: "value（输入框文本值）"
        }
      ],
      data20: [
        {
          name: "text",
          desc: "按钮默认文本",
          type: "String",
          value: "提交"
        },
        {
          name: "autoCountDown",
          desc: "点击后即自动开始倒计时，设置为false后可手动触发倒计时",
          type: "Boolean",
          value: "true"
        },
        {
          name: "countTime",
          desc: "倒计时时间，单位：秒",
          type: "Number",
          value: "60"
        },
        {
          name: "suffixText",
          desc: "倒计时中文本后缀，如'60秒后重试'，其中‘后重试’可自定义",
          type: "String",
          value: "后重试"
        },
        {
          name: "type",
          desc:
            "按钮类型，可选值为 default、primary、dashed、text、info、success、warning、error或者不设置",
          type: "String",
          value: "default"
        },
        {
          name: "ghost",
          desc: "幽灵属性，使按钮背景透明",
          type: "Boolean",
          value: "false"
        },
        {
          name: "size",
          desc: "按钮大小，可选值为large、small、default或者不设置",
          type: "String",
          value: "default"
        },
        {
          name: "shape",
          desc: "按钮形状，可选值为circle或者不设置",
          type: "String",
          value: "-"
        },
        {
          name: "long",
          desc: "开启后，按钮的长度为100%",
          type: "Boolean",
          value: "false"
        },
        {
          name: "disabled",
          desc: "设置按钮为禁用状态",
          type: "Boolean",
          value: "false"
        },
        {
          name: "loading",
          desc: "设置按钮为加载中状态",
          type: "Boolean",
          value: "false"
        },
        {
          name: "icon",
          desc: "设置按钮的图标类型",
          type: "String",
          value: "-"
        }
      ],
      data21: [
        {
          name: "startCountDown",
          type: "当autoCountDown设置为false时生效，手动开启倒计时",
          value: "无"
        }
      ],
      data22: [
        {
          name: "on-click",
          type: "用户点击事件",
          value: "无"
        }
      ],
      data23: [
        {
          name: "multiple",
          desc: "是否选开启多张上传，默认true开启，设为false仅限制一张",
          type: "Boolean",
          value: "true"
        },
        {
          name: "limit",
          desc: "限制上传数量，开启多张上传multiple设为true时生效，默认限制10张",
          type: "Number",
          value: "10"
        }
      ],
      data24: [
        {
          name: "on-change",
          type: "返回上传成功图片链接",
          value:
            "当开启多张上传时，返回图片链接数组，如['http://1.png','http://2.png']；限制单张时返回单个图片链接，如'http://3.png'"
        }
      ],
      data25: [
        {
          name: "setData",
          type: "设置图片链接值（回显使用）",
          value:
            "根据多张上传配置，传入多张图片链接数组或单张图片链接，如['http://1.png','http://2.png']或'http://3.png'"
        }
      ],
      data26: [
        {
          name: "on-success",
          type: "仅当验证密码正确触发回调",
          value: "true"
        }
      ],
      data27: [
        {
          name: "show",
          type: "显示密码验证组件",
          value: "无"
        }
      ]
    };
  },
  methods: {
    init() {},
    showLoading() {
      this.$store.commit("setLoading", true);
    },
    closeLoading() {
      this.$store.commit("setLoading", false);
    },
    start() {
      this.$Loading.start();
    },
    finish() {
      this.$Loading.finish();
    },
    error() {
      this.$Loading.error();
    },
    handleSelectDep(v) {
      this.$Message.info(`所选部门ID为 ${v}`);
    },
    handleSelectUser(v) {
      this.selectUsers = v;
    },
    selectAllUser() {
      this.$refs.user.setSelectAllUser();
    },
    handleSelectDepTree(v) {
      this.selectDeps = v;
    },
    showCheckPass() {
      this.$refs.checkPass.show();
    },
    checkSuccess() {
      this.$Message.success("验证成功");
    }
  },
  mounted() {
    this.init();
  }
};
</script>

<style lang="less">
.article {
  font-size: 16px;
  font-weight: 400;
  margin: 12px 0;
}
.blue {
  color: #40a9ff !important;
}
code {
  display: inline-block;
  background: #f7f7f7;
  font-family: Consolas, Monaco, Andale Mono, Ubuntu Mono, monospace;
  margin: 0 3px;
  padding: 1px 5px;
  border-radius: 3px;
  color: #666;
  border: 1px solid #eee;
}
.sorry {
  height: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  .text {
    font-size: 20px;
    margin: 20px 0;
  }
}
</style>