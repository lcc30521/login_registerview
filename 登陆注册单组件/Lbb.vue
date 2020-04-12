<template>
<!--注册-->
<div> 
<span @click="login()" style="margin-right:12px;">登录</span>
<span @click="registera()">注册</span>
<el-dialog  :visible.sync="dialogFormVisible" :modal-append-to-body="false" width="40%" center>
  <img class="modal-logo" src="/images/logo.png">

<!--注册逻辑start-->
<div  v-show="isregister">
  <ul class="ant-menu ant-menu-light ant-menu-root ant-menu-horizontal" role="menu">
    <li class="ant-menu-submenu ant-menu-submenu-horizontal ant-menu-overflowed-submenu" role="menuitem" style="display: none;">
      <div class="ant-menu-submenu-title" aria-expanded="false" aria-haspopup="true">
        <span>···</span>
        <i class="ant-menu-submenu-arrow"></i></div>
        </li>
        <li  @click="cur=0"   :class="{antmenuitemselected:cur==0} " id="antmenuitem" role="menuitem">手机注册</li>
        <li class="ant-menu-submenu ant-menu-submenu-horizontal ant-menu-overflowed-submenu" role="menuitem" style="display: none;">
          <div class="ant-menu-submenu-title" aria-expanded="false" aria-haspopup="true">
            <span>···</span>
            <i class="ant-menu-submenu-arrow"></i>
            </div></li>
            <li @click="cur=1" :class="{antmenuitemselected:cur==1}" id="antmenuitem" role="menuitem">微信注册</li>
            <li class="ant-menu-submenu ant-menu-submenu-horizontal ant-menu-overflowed-submenu" role="menuitem" style="visibility: hidden; position: absolute; display: none;">
              <div class="ant-menu-submenu-title" aria-expanded="false" aria-haspopup="true">
    <span>···</span><i class="ant-menu-submenu-arrow"></i></div></li>
    </ul>
   <!--表单主体--> 
  <div v-show="cur==0">
  <el-form :model="form" width="60%" :rules="rules" ref="form">
     <el-form-item prop="name">
      <el-input v-model="form.name" autocomplete="off" placeholder="昵称 请输入3~6个字符" prefix-icon="el-icon-mobile"></el-input>
    </el-form-item>
    <el-form-item prop="phone">
      <el-input v-model.number="form.phone" autocomplete="off" placeholder="手机号码" prefix-icon="el-icon-mobile"></el-input>
    </el-form-item>
     <el-form-item prop="verify">
      <el-input v-model="form.verify" autocomplete="off" placeholder="输入验证码" class="very"></el-input>
      <el-button type="success" @click="sendcode" :disabled="disabled" v-if="disabled==false">获取验证码</el-button>
      <el-button type="success" @click="sendcode" :disabled="disabled" v-if="disabled==true">{{btntxt}}
      </el-button>
    </el-form-item>
     <el-form-item prop="pass">
      <el-input v-model="form.pass" autocomplete="off" placeholder="密码" prefix-icon="el-icon-lock" type="password"></el-input>
    </el-form-item>
    <el-form-item  prop="checkPass">
    <el-input type="password" v-model="form.checkPass" autocomplete="off" placeholder="请确认密码" prefix-icon="el-icon-lock"></el-input>
  </el-form-item>
  
  </el-form>
  <!--阅读协议-->
  <div class="yuedu mb-20">
    <div :class="{'yuedudiva':ins,'yuedudiv':1} " @click="active()">
      <label >
        <a class="text-primary" href="/terms">
          已阅读协议</a>
      </label>
    </div>
  </div>
  <!--阅读协议-->
  <div slot="footer" class="dialog-footer">
    <el-button type="primary" @click="submitForm('form')" width="80%;" :disabled="isAble" :class="{'buttongray':lcc}">立即注册</el-button>
  </div>
   <!--已有账号  立即登录-->
  <div class="mb-20" style="text-align:center;margin-top:24px;">
    <span class=" mr-10">已有账号</span>
    <a href="javascript:;" class="text-primary " @click="aoc()">去登录</a>
  </div>
  <!--已有账号 立即登录-->
  </div>
  <!--表单主体-->
  <div v-show="cur==1">
     <span>微信注册待开发</span>
  </div>
  </div>
  <!--注册逻辑end-->
  <div v-show="!isregister">
     <ul class="ant-menu ant-menu-light ant-menu-root ant-menu-horizontal" role="menu">
    <li class="ant-menu-submenu ant-menu-submenu-horizontal ant-menu-overflowed-submenu" role="menuitem" style="display: none;">
      <div class="ant-menu-submenu-title" aria-expanded="false" aria-haspopup="true">
        <span>···</span>
        <i class="ant-menu-submenu-arrow"></i></div>
        </li>
        <li  @click="cur_login=0"   :class="{antmenuitemselected:cur_login==0} " id="antmenuitem" role="menuitem">手机登录</li>
        <li class="ant-menu-submenu ant-menu-submenu-horizontal ant-menu-overflowed-submenu" role="menuitem" style="display: none;">
          <div class="ant-menu-submenu-title" aria-expanded="false" aria-haspopup="true">
            <span>···</span>
            <i class="ant-menu-submenu-arrow"></i>
            </div></li>
            <li @click="cur_login=1" :class="{antmenuitemselected:cur_login==1}" id="antmenuitem" role="menuitem">微信登录</li>
            <li class="ant-menu-submenu ant-menu-submenu-horizontal ant-menu-overflowed-submenu" role="menuitem" style="visibility: hidden; position: absolute; display: none;">
              <div class="ant-menu-submenu-title" aria-expanded="false" aria-haspopup="true">
    <span>···</span><i class="ant-menu-submenu-arrow"></i></div></li>
    </ul>
      <!--表单主体--> 
  <div v-show="cur_login==0">
  <el-form :model="formlogin" width="60%" :rules="ruleslogin" ref="formlogin">
    <el-form-item prop="phonelogin">
      <el-input v-model="formlogin.phonelogin" autocomplete="off" placeholder="手机号码" prefix-icon="el-icon-mobile"></el-input>
    </el-form-item>
     <el-form-item prop="passlogin">
      <el-input v-model="formlogin.passlogin" autocomplete="off" placeholder="密码" prefix-icon="el-icon-lock" type="password"></el-input>
    </el-form-item>
  </el-form>
  
  <div slot="footer" class="dialog-footer">
    <el-button type="primary" @click="loginsubmitForm('formlogin')" width="80%;" >立即登录</el-button>
  </div>
  <!--mei有账号  立即注册-->
  <div class="mb-20" style="text-align:center;margin-top:24px;">
    <span class=" mr-10">没有账号</span>
    <a href="javascript:;" class="text-primary " @click="aoc()">去注册</a>
  </div>
  <!--已有账号 立即登录-->
  </div>
  </div>

  <!--登录逻辑start-->

  <!--登录逻辑end-->
</el-dialog>
</div>
</template>

<script>
  export default {
    data() {
       //手机号码验证
        var checkPhone = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('手机号不能为空'));
        } else {
          const reg = /^1[3|4|5|7|8][0-9]\d{8}$/
          console.log(reg.test(value));
          if (reg.test(value)) {
            callback();
          } else {
            return callback(new Error('请输入正确的手机号'));
          }
        }
        };
       //手机号码验证
       //密码验证
        var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.form.checkPass !== '') {
            this.$refs.form.validateField('checkPass');
          }
          callback();
        }
      };
       //密码验证
       //再次输入密码验证
       var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.form.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
       //再次输入密码验证

      return {
        dialogTableVisible: false,
        dialogFormVisible: false,
        form: {
          phone: '',
          pass: '',
          verify:'',
          checkPass:'',
          name:'',
        },
        formlogin: {
          phonelogin: '',
          passlogin: '',
        },
        cur:0,
        cur_login:0,
        ins:0,
        isAble:false,
        formLabelWidth: '120px',
        disabled: false,
        time: 0,
        lcc:0,
        isregister:true,
        btntxt: "重新发送",
        //登录验证规则
        ruleslogin:{
           phonelogin: [
            { validator: checkPhone, trigger: 'blur' }
          ],
           passlogin: [
            { validator: validatePass, trigger: 'blur' }
          ]
        },
        //登录验证规则
        //表单验证规则
        rules: {
           name: [
             { required: true, message: '昵称不能为空', trigger: 'blur' },
          ],
          pass: [
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          phone: [
            { validator: checkPhone, trigger: 'blur' }
          ],
          verify: [
             { required: true, message: '验证码不能为空', trigger: 'blur' },
          ]
          
        }
        //表单验证规则
      };
    },
    methods: {
      //点击登录选项
      login(){
         this.dialogFormVisible=true;
        this.isregister=false;
      },
      //点击注册选项
      registera(){
            this.dialogFormVisible=true;
        this.isregister=true;
      },
      aoc(){
        this.isregister=!this.isregister;
      },
      active(){
         this.ins =!this.ins;
         if(this.ins==1){
           this.isAble = true;
           this.lcc = 1;
         }else{
           this.isAble =false;
           this.lcc =0;
         }
      },
      //手机发送验证码
       sendcode() {
                const reg = 11 && /^((13|1form4|15|17|18)[0-9]{1}\d{8})$/
                if (this.form.phone == '') {
                    this.$message({
                        message:'手机号不能为空',
                        type:'error',
                        center: true
                    })
                    return
                }
                if (!reg.test(this.form.phone)) {
                    this.$message({
                        message:'请输入正确的手机号',
                         type:'error',
                        center:true
                    })
                    return
                } else {
                   //发送短信验证码
                   //提交表单注册
                  axios.post('/url',{
                      phone:this.form.phone
                  }).then((response)=>{
                      var ls = response.data.tag;
                      if(ls==1){
                          this.$message({
                              message: '发送成功',
                              type: 'success',
                              center:true
                          });
                      }else if(ls==3){
                          this.$message({
                              message: '该手机号码已被注册',
                              type: 'error',
                              center:true
                          });
                      }
                      else if(ls==4){
                          this.$message({
                              message: '请勿频繁获取短信验证码',
                              type: 'waring',
                              center:true
                          });
                      }
                      else{
                        this.$message({
                              message: '短信验证码发送失败 请稍后重试',
                              type: 'error',
                              center:true
                          });
                      }
                     
                  }).catch((error)=>{
                    this.$message({
                              message: '注册失败 请稍后重试',
                              type: 'error',
                              center:true
                          });
                  })
                        //发送短信验证码
                          this.time = 60;
                          this.disabled = true;
                          this.timer();
                      }
                  },
          
      //手机发送验证码
      //倒计时
       //60S倒计时
            timer() {
                if (this.time > 0) {
                    this.time--;
                    this.btntxt = this.time + "s后重新获取";
                    setTimeout(this.timer, 1000);
                } else {
                    this.time = 0;
                    this.btntxt = "获取验证码";
                    this.disabled = false;
                }
            },

      //倒计时
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            //提交表单注册
            axios.post('/url',{
                phone:this.form.phone,
                pass:this.form.pass,
                name:this.form.name,
                verify:this.form.verify,
            }).then((response)=>{
               var ts = response.data.tag;
               console.log(response);
               if(ts==1){
                  this.$message({
                        message: '注册成功',
                        type: 'success',
                        center:true
                    });
                   setTimeout("location.reload();",2000);
               }else if(ts==2){
                  this.$message({
                        message: '短信验证码已经过期',
                        type: 'error',
                        center:true
                    });
               }
               else if(ts==3){
                  this.$message({
                        message: '该手机号码已经被注册',
                        type: 'error',
                        center:true
                    });
               }
                else if(ts==4){
                    this.$message({
                        message: '请先获取手机验证码',
                        type: 'error',
                        center:true
                    });
               }
                 else if(ts==5){
                    this.$message({
                        message: '手机验证码输入错误',
                        type: 'error',
                        center:true
                    });
               }
               else{
                 this.$message({
                        message: '网络繁忙  稍后重试',
                        type: 'error',
                        center:true
                    });
               }
               

                
            }).catch((error)=>{
              if(error.response){
                    var nameerrors = error.response.data.errors.name;
                    console.log(nameerrors);
                    for(var j = 0,len=nameerrors.length; j < len; j++) {
                          this.$message({
                              message: nameerrors[j],
                              type: 'error',
                              center:true
                          });
                      }
              }else{
                this.$message({
                        message: '注册失败 请稍后重试',
                        type: 'error',
                        center:true
                    });
              }
              
              
            })
           // alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      loginsubmitForm(formName){
         this.$refs[formName].validate((valid) => {
          if (valid) {
                      axios.post('/url',{
                      phone:this.formlogin.phonelogin,
                      password:this.formlogin.passlogin  
                  }).then((response)=>{   //成功状态
                     this.$message({
                        message: '登录成功',
                        type: 'success',
                        center:true
                    });
                      setTimeout("location.reload();",1000);
                  }).catch((error)=>{
                     this.$message({
                        message: '手机号或者密码错误',
                        type: 'error',
                        center:true
                    });
                  })

          }else {
            console.log('error submit!!');
            return false;
          }
      });
      }
    }
  };
</script>

<style scoped>
*, *::before, *::after {
    box-sizing: border-box;
}
.el-dialog{
      margin-top: 8vh!important;
}
.mb-20{
  margin-bottom:20px;
}
input[type='checkbox'] {
    width: 14px;
    height: 14px;
  }
.yuedudiv {
    cursor: pointer;
    display: inline-block;
    height: 20px;
    background-image: url(/images/ass.png);
    //filter: grayscale(100%);
    padding-left: 20px;
    vertical-align: middle;
    font-size: 14px;
    background-repeat: no-repeat;
    background-position: left center;
}
.yuedudiva {
    cursor: pointer;
    display: inline-block;
    height: 20px;
    background-image: url(/images/ass.png);
    filter: grayscale(100%);
    padding-left: 20px;
    vertical-align: middle;
    font-size: 14px;
    background-repeat: no-repeat;
    background-position: left center;
}
.buttongray{
  filter: grayscale(100%);
}
a.text-primary {
    color: #67C23A;
    font-family: PingFangSC-Regular, PingFangSC;
    font-weight: 300;
    text-decoration:none;
}
.el-button--primary{
  width:100% !important;
  background-color:#67C23A !important;
 border-color: #67C23A !important;
}
  .very{
    width:60%;
  }
  .success{
    width:40%;
  }
  input:focus{
        border: 1px solid #b73b3b;
  }
  .modal-logo {
    display: block;
    margin: 17px auto;
    height: 38px;
}

.ant-menu-horizontal {
    line-height: 46px;
    white-space: nowrap;
    border: 0;
    border-bottom: 1px solid #e8e8e8;
    box-shadow: none;
}
.ant-menu {
   
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-size: 14px;
    /* font-variant: tabular-nums; */
    line-height: 1.5;
    font-feature-settings: 'tnum';
    margin-bottom: 0;
    padding-left: 0;
    color: rgba(0, 0, 0, 0.65);
    line-height: 0;
    list-style: none;
    background: #fff;
    outline: none;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
    transition: background 0.3s, width 0.2s;
    zoom: 1;
     text-align: center;
    font-size: 18px;
    margin-bottom: 15px!important;
}
.ant-menu-submenu {
    position: relative;
    top: 1px;
    display: inline-block;
    vertical-align: bottom;
    border-bottom: 2px solid transparent;
}
.ant-menu-submenu, .ant-menu-submenu-inline {
    transition: border-color 0.3s cubic-bezier(0.645, 0.045, 0.355, 1), background 0.3s cubic-bezier(0.645, 0.045, 0.355, 1), padding 0.15s cubic-bezier(0.645, 0.045, 0.355, 1);
}
#antmenuitem, .ant-menu-submenu-title {
    position: relative;
    display: block;
    margin: 0;
    padding: 0 20px;
    white-space: nowrap;
    cursor: pointer;
    transition: color 0.3s cubic-bezier(0.645, 0.045, 0.355, 1), border-color 0.3s cubic-bezier(0.645, 0.045, 0.355, 1), background 0.3s cubic-bezier(0.645, 0.045, 0.355, 1), padding 0.15s cubic-bezier(0.645, 0.045, 0.355, 1);
}
.antmenuitemselected {
    color: #67C23A;
    border-bottom: 2px solid #67C23A !important;
}
.ant-menu-horizontal > #antmenuitem, .ant-menu-horizontal > .ant-menu-submenu {
    position: relative;
    top: 1px;
    display: inline-block;
    vertical-align: bottom;
    border-bottom: 2px solid transparent;
}
.ant-menu-horizontal {
    line-height: 46px;
    white-space: nowrap;
    border: 0;
    border-bottom: 1px solid #e8e8e8;
    box-shadow: none;
}
.el-dialog--center .el-dialog__body {
    text-align: initial;
    padding: 22px 34px 30px !important;
}
</style>

