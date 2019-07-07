<template>
  <div id="detail">
    <mt-header title="用户信息">
      <router-link to="/me" slot="left">
        <mt-button icon="back">返回</mt-button>
      </router-link>
      <mt-button icon="more" slot="right"></mt-button>
    </mt-header>
    <mt-field label="用户名" placeholder="用户名" v-model="username"></mt-field>
    <mt-field label="头像" placeholder="头像" v-model="userimg"></mt-field>
    <mt-field label="邮箱" placeholder="email" type="email" v-model="email"></mt-field>
    <mt-field label="密码" placeholder="密码" type="text" v-model="password"></mt-field>
    <mt-field label="性别"  placeholder type="text" v-model="sex"></mt-field>
    <mt-field label="年龄" placeholder type="text" v-model="age"></mt-field>
    <mt-field label="电话" placeholder="电话" type="tel" v-model="phone"></mt-field>
    <mt-field label="家庭地址" placeholder="家庭地址" v-model="address"></mt-field>
    <mt-field label="个性签名" placeholder="个性签名" type="textarea" rows="4" v-model="introduction"></mt-field>
    <mt-button class="sureBtn" type="danger" @click="updateUserInfo" size="large">确认修改</mt-button>
  </div>
</template>

<script>
import { Indicator } from "mint-ui";
import { Toast } from "mint-ui";
export default {
  data() {
    return {
      userinfo: {},
      username: "",
      userimg: "",
      email: "",
      password: "",
      phone: "",
      introduction: "",
      address: "",
      sex: "",
      age: "",
      userID: JSON.parse(this.$store.state.login.userID)
    };
  },
  created() {
    this.getuserinfo();
  },
  methods: {
    //   获取用户信息
    getuserinfo() {
      this.userinfo = JSON.parse(window.localStorage.getItem("userinfo"));
      // console.log(this.userinfo);
      this.username = this.userinfo.user_name;
      this.userimg =
        this.userinfo.user_img ||
        "http://img2.imgtn.bdimg.com/it/u=252376943,2755465431&fm=26&gp=0.jpg";
      this.email = this.userinfo.user_email || "未设置";
      this.password = this.userinfo.user_pass || "未设置";
      this.phone = this.userinfo.user_phone || "未设置";
      this.address = this.userinfo.user_address || "未设置";
      (this.sex = this.userinfo.user_sex || "男"),
        (this.age = this.userinfo.user_age || 0),
        (this.introduction = this.userinfo.user_auto || "还没有说说哦~~");
    },
    updateUserInfo() {
      Indicator.open("Loading...");
      this.axios
        .post("http://localhost:3000/api/updateuserinfo", {
          name: this.username,
          img: this.userimg,
          email: this.email,
          pass: this.password,
          phone: this.phone,
          address: this.address,
          auto: this.introduction,
          sex: this.sex,
          age: this.age,
          id: this.userID
        })
        .then(res => {
          // console.log(res);
          if (res.data.success_code === 200) {
            Toast({
              message: res.data.message,
              position: "middle",
              duration: 500
            });
            Indicator.close();
            window.localStorage.setItem("userinfo", this.userinfo);
            this.$router.replace("/me");
          } else {
            Toast({
              message: res.data.message,
              position: "middle",
              duration: 1000
            });
          }
        });
    }
  }
};
</script>
<style  scoped>
#detail {
  width: 100%;
  height: 100%;
  background: #f5f5f5;
}
.sureBtn {
  width: 85%;
  margin: 20px auto;
}
</style>