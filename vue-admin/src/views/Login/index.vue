<template>
  <div id="login">
    这里是登陆页
    <div class="login-warp">
      <ul class="menu-tab">
        <li
          :class="{'current':item.current}"
          v-for="item in menuTab"
          :key="item.id"
          @click="toggleMenu(item)"
        >{{item.txt}}</li>
      </ul>
      <!-- 表单 -->
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        class="login-form"
        size = "medium"
      >
        <el-form-item  prop="username">
          <label>邮箱</label>
          <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item  prop="password">
          <label>密码</label>
          <el-input type="password" v-model="ruleForm.password" autocomplete="off" minlength="6" maxlength="20"></el-input>
        </el-form-item>

        <el-form-item  prop="code">
        <label>验证码</label>
        <el-row :gutter="11">
            <el-col :span="15"><el-input v-model.number="ruleForm.code" minlength="6" maxlength="6"></el-input></el-col>
            <el-col :span="9"><el-button type="success" class="block" >获取验证码</el-button></el-col>
        </el-row>
          
        </el-form-item>

        <el-form-item>
          <el-button type="danger" @click="submitForm('ruleForm')" class="login-btn block">提交</el-button>

        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  name: "login",
  data() { 
      //用户名
      var validateUsername = (rule, value, callback) => {
        let reg=/^([a-zA-Z]|[0-9])(\w|-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
        if(value === '') {
          callback(new Error('请输入用户名'));
        } else if(!reg.test(value)){
            callback(new Error('请输入正确的用户名'));
        }  else {
          callback();
        }
      };
      //密码
      var validatePassword = (rule, value, callback) => {
        let reg=/^(?!\D+$)(?![^a-zA-Z]+$)\S{6,20}$/
        if (value === '') {
          callback(new Error('请输入密码'));
        } else if (reg.test(value)) {
          callback(new Error('密码为6-20位的数字加字母'));
        } else {
          callback();
        }
      };
      //验证码
      var validateCode = (rule, value, callback) => {
        let reg=/^[a-z0-9]{6}$/
        if (!value) {
          return callback(new Error('请输入验证码'));
        }else if(reg.text(value)){
            return callback(new Error('格式有误'));
        }else{
            callback();
        }
      };
    return {
      menuTab: [
        { txt: "登陆", current: false },
        { txt: "注册", current: true }
      ],
      //表单数据
      ruleForm: {
          username: '',
          password: '',
          code: ''
        },
        rules: {
          username: [
            { validator: validateUsername, trigger: 'blur' }
          ],
          password: [
            { validator: validatePassword, trigger: 'blur' }
          ],
          code: [
            { validator: validateCode, trigger: 'blur' }
          ]
        }
    };
  },
  methods: {
    toggleMenu(data) {
      this.menuTab.forEach(elem => {
        elem.current = false;
      });
      data.current = true;
    },
    submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      }
  }
};
</script> <style lang="scss" scoped>
#login {
  background-color: #344a5f;

  height: 100vh;
}

.login-warp {
  width: 330px;

  margin: auto;
}

.menu-tab {
  text-align: center;
  padding: 0;

  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #ffffff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
.login-form{
    margin-top:29px;
    label{
        display: block;
        font-size: 14px;
        color: white;
    }
    .block{
        width: 100%;
        display: block;
    }
    .login-btn{
        margin-top:19px;
    }
}
</style>