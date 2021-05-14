<template>
  <div id="app">
    <div class="button">
      <h2>按钮</h2>
      <div class="row">
        <h3>普通按钮</h3>
        <yh-button>button</yh-button>
        <yh-button type="primary">primary</yh-button>
        <yh-button type="success">success</yh-button>
        <yh-button type="info">info</yh-button>
        <yh-button type="warning">warning</yh-button>
        <yh-button type="danger">danger</yh-button>
      </div>
      <div class="row">
        <h3>拟态按钮</h3>
        <yh-button mimicry>button</yh-button>
        <yh-button type="primary" mimicry>primary</yh-button>
        <yh-button type="success" mimicry>success</yh-button>
        <yh-button type="info" mimicry>info</yh-button>
        <yh-button type="warning" mimicry>warning</yh-button>
        <yh-button type="danger" mimicry>danger</yh-button>
      </div>

      <div class="row">
        <h3>朴素按钮</h3>
        <yh-button plain>button</yh-button>
        <yh-button type="primary" plain>primary</yh-button>
        <yh-button type="success" plain>success</yh-button>
        <yh-button type="info" plain>info</yh-button>
        <yh-button type="warning" plain>warning</yh-button>
        <yh-button type="danger" plain>danger</yh-button>
      </div>

      <div class="row">
        <h3>圆角按钮</h3>
        <yh-button round>button</yh-button>
        <yh-button type="primary" round>primary</yh-button>
        <yh-button type="success" round>success</yh-button>
        <yh-button type="info" round>info</yh-button>
        <yh-button type="warning" round>warning</yh-button>
        <yh-button type="danger" round>danger</yh-button>
      </div>
      <div class="row">
        <h3>圆形按钮</h3>
        <yh-button circle :icon="['far', 'smile']"> </yh-button>
        <yh-button type="primary" circle :icon="['far', 'edit']"> </yh-button>
        <yh-button type="success" circle :icon="['fas', 'check']"> </yh-button>
        <yh-button type="info" circle :icon="['far', 'envelope']"> </yh-button>
        <yh-button type="warning" circle :icon="['far', 'star']"> </yh-button>
        <yh-button type="danger" circle :icon="['far', 'trash-alt']">
        </yh-button>
      </div>
      <div class="row">
        <h3>禁用按钮</h3>
        <yh-button plain disabled>button</yh-button>
        <yh-button type="primary" disabled>primary</yh-button>
        <yh-button type="success" disabled>success</yh-button>
        <yh-button type="info" disabled>info</yh-button>
        <yh-button type="warning" disabled>warning</yh-button>
        <yh-button type="danger" disabled>danger</yh-button>
      </div>
      <div class="row">
        <h3>文字加图标按钮</h3>
        <yh-button :icon="['far', 'smile']">button</yh-button>
        <yh-button type="primary" :icon="['far', 'edit']">primary</yh-button>
        <yh-button type="success" :icon="['fas', 'check']">success</yh-button>
        <yh-button type="info" :icon="['far', 'envelope']">info</yh-button>
        <yh-button type="warning" :icon="['far', 'star']">warning</yh-button>
        <yh-button type="danger" :icon="['far', 'trash-alt']">danger</yh-button>
      </div>
      <div class="row">
        <h3>点击事件</h3>
        <yh-button type="primary" @click="sayHello">say hello</yh-button>
      </div>
    </div>
    <div class="dialog">
      <yh-button type="primary" @click="dialogVisiable = true"
        >显示对话框</yh-button
      >
      <yh-dialog title="tip" top="200px" :visiable.sync="dialogVisiable">
        <template v-slot:title>
          <span>提示</span>
        </template>

        <span>这是一段信息</span>
        <template v-slot:footer>
          <yh-button @click="dialogVisiable = false">取消</yh-button>
          <yh-button type="primary" @click="dialogVisiable = false"
            >确定</yh-button
          >
        </template>
      </yh-dialog>
    </div>
    <div class="input">
      <h2>输入框</h2>
      <div class="row">
        <h3>基本输入框</h3>
        <yh-input placeholder="请输入内容"></yh-input>
      </div>
      <div class="row">
        <h3>文本域输入框</h3>
        <yh-input placeholder="写点什么" type="textarea"></yh-input>
      </div>
      <div class="row">
        <h3>禁用</h3>
        <yh-input placeholder="请输入内容" disabled></yh-input>
      </div>
      <div class="row">
        <h3>可清除</h3>
        <yh-input
          placeholder="请输入内容"
          type="text"
          name="username"
          v-model="username"
          clearable
        ></yh-input>
      </div>
      <div class="row">
        <h3>密码可见</h3>
        <yh-input
          placeholder="请输入内容"
          type="password"
          name="password"
          v-model="password"
          show-password
        ></yh-input>
      </div>
    </div>
    <!-- v-model相当于:value=xxx和@input="xxx=e.target.value" -->
    <div class="switch">
      <h2>Switch</h2>
      <div class="row">
        <h3>基本Switch</h3>
        <yh-switch v-model="switchActive"></yh-switch>
      </div>
      <div class="row">
        <h3>自定义颜色</h3>
        <yh-switch
          v-model="switchActive"
          activeColor="skyblue"
          inactiveColor="orange"
          name="paied"
        ></yh-switch>
        <div class="row">
          <h3>禁用</h3>
          <yh-switch v-model="switchActive" disabled></yh-switch>
        </div>
        <div class="row">
          <h3>拟态</h3>
          <yh-switch v-model="switchActive" mimicry></yh-switch>
        </div>
        <div class="row">
          <h3>change事件</h3>
          <yh-switch
            v-model="switchActive"
            activeColor="skyblue"
            @change="handleChange"
          ></yh-switch>
        </div>
      </div>
    </div>
    <div class="radio">
      <h2>单选框</h2>
      <div class="row">
        <h3>基本样式</h3>
        <yh-radio label="1" v-model="gender" name="male">男</yh-radio>
        <yh-radio label="2" v-model="gender" name="female">女</yh-radio>
      </div>
      <div class="row">
        <h3>自定义颜色</h3>
        <yh-radio label="1" v-model="gender" name="male" color="green"
          >男</yh-radio
        >
        <yh-radio label="2" v-model="gender" name="female" color="pink"
          >女</yh-radio
        >
      </div>
      <div class="row">
        <h3>拟态</h3>
        <yh-radio label="1" v-model="gender" name="male" mimicry>男</yh-radio>
        <yh-radio label="2" v-model="gender" name="female" mimicry>女</yh-radio>
      </div>
      <div class="row">
        <h3>radio组</h3>
        <yh-radio-group v-model="gender">
          <yh-radio label="1" name="male">男</yh-radio>
          <yh-radio label="2" name="female">女</yh-radio>
        </yh-radio-group>
      </div>
    </div>
    <div class="checkout">
      <h2>复选框</h2>
      <div class="row">
        <h3>基础样式</h3>
        <yh-checkbox
          name="read"
          label="1"
          v-model="checkboxChoice"
        ></yh-checkbox>
      </div>

      <div class="row">
        <h3>自定义颜色</h3>
        <yh-checkbox
          name="read"
          label="1"
          v-model="checkboxChoice"
          activeColor="orange"
        ></yh-checkbox>
      </div>
      <div class="row">
        <h3>拟物样式</h3>
        <yh-checkbox
          name="read"
          label="1"
          v-model="checkboxChoice"
          mimicry
        ></yh-checkbox>
      </div>
      <div class="row">
        <h3>禁用</h3>
        <yh-checkbox
          name="read"
          label="1"
          v-model="checkboxChoice"
          disabled
        ></yh-checkbox>
      </div>
    </div>
    <div class="checkboxGroup">
      <h2>复选框组</h2>
      <div class="row">
        <yh-checkbox-group v-model="checkedArray">
          <yh-checkbox name="apple" label="1">苹果</yh-checkbox>
          <yh-checkbox name="banana" label="2">香蕉</yh-checkbox>
          <yh-checkbox name="peach" label="3">桃子</yh-checkbox>
        </yh-checkbox-group>
      </div>
    </div>

    <div class="formItem">
      <h2>表单项</h2>
      <!-- <div class="row">
        <yh-form-item label="活动">
          <yh-input name="username"></yh-input>
        </yh-form-item>
      </div> -->
      <yh-form :model="form" label-width="80px">
        <yh-form-item label="用户名">
          <yh-input name="username" v-model="form.name"></yh-input>
        </yh-form-item>
        <yh-form-item label="邮箱">
          <yh-input name="email" v-model="form.email"></yh-input>
        </yh-form-item>
      </yh-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisiable: false,
      username: 'honyau',
      password: '123456',
      switchActive: false,
      gender: '1',
      checkboxChoice: false,
      checkedArray: ['1', '3'],
      form: {
        name: 'honyau',
        email: 'honyau@mail.com',
        labelwidth: '80px',
      },
    }
  },
  methods: {
    sayHello() {
      window.alert('hello')
    },
    closeDialog(value) {
      this.dialogVisiable = value
    },
    handleChange(value) {
      alert(`change switch to ${value}`)
    },
  },
}
</script>

<style lang="scss">
.row {
  margin-bottom: 20px;
  .yh-button {
    margin-right: 10px;
  }
}
.input {
  & .yh-input {
    width: 18vw;
  }
}

.formItem {
  .yh-input {
    width: 20vw;
  }
}
</style>
