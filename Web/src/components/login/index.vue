<template>
  <div class="user-login">
    <el-input placeholder="User ID" v-model="UserID" maxlength="11" class="phone-num"></el-input>
    <el-button class="user-login-btn" @click="handleLogin">Log in</el-button>
  </div>
</template>

<script>
import { genTestUserSig } from "../../../public/debug/GenerateTestUserSig";

export default {
  name: "Login",
  data() {
    return {
      UserID: "",
      verifyCode: "",
      disableFetchCodeBtn: false
    };
  },
  methods: {
    handleLogin: async function() {
      console.log("userid");
      if (!this.UserID) {
        this.$message.error("Enter a user ID");
        return;
      }

      const userSig = genTestUserSig(this.UserID).userSig;
      const userId = this.UserID;
      this.$store.commit("userLoginSuccess");
      this.$store.commit("setLoginUserInfo", {
        userId,
        userSig
      });

      // Log in to `trtcCalling`
      // 登录 trtcCalling
      this.$trtcCalling.login({
        userID: this.UserID,
        userSig
      });
    }
  }
};
</script>

<style scoped>
.user-login {
  font-size: 16px;
  width: 400px;
  margin: 0 auto;
  padding-top: 50px;
}
.phone-num {
  margin-bottom: 5px;
}
.user-login-btn {
  margin-top: 10px;
  width: 100%;
}
@media screen and (max-width: 767px) {
  .user-login {
    font-size: 16px;
    width: 90%;
    min-width: 300px;
    margin: 0 auto;
    padding-top: 50px;
  }
}
</style>
