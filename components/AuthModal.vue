<template>
  <section class="container">
    <div class="user-login">
      <form class="b-box" @submit="login">
        <a href="https://quwi.com/" class="c-quwi-logo"
          ><img width="40px" height="40px" :src="image" />
          <div class="e-logo-text">QUWI</div></a
        >
        <CustomInput
          :value="email.value"
          :type="email.type"
          :placeholder="email.placeholder"
          :error="email.error"
          :errorText="email.errorText"
          @change="emailHandler"
        />
        <CustomInput
          :value="password.value"
          :type="password.type"
          :placeholder="password.placeholder"
          :error="password.error"
          :errorText="password.errorText"
          @change="passwordHandler"
        />
        <div class="b-button">
          <button type="submit" class="btn" :disabled="onLogin">Login</button>
          <a href="/" class=""> Forgot password? </a>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
import image from "../img/quwi-logo.png";
import axios from "axios";

export default {
  name: "AuthModal",
  data() {
    return {
      email: {
        value: "",
        type: "text",
        error: false,
        errorText: "Email can not be blank",
        placeholder: "email",
      },
      password: {
        value: "",
        type: "password",
        error: false,
        errorText: "Password can not be blank",
        placeholder: "password",
      },
      image,
      onLogin: false,
    };
  },
  methods: {
    emailHandler(value) {
      if (this.email.error) {
        this.email.error = false;
      }
      if (this.password.error) {
        this.password.error = false;
      }
      this.email.value = value;
    },
    passwordHandler(value) {
      if (this.password.error) {
        this.password.error = false;
      }
      if (this.email.error) {
        this.email.error = false;
      }
      this.password.value = value;
    },

    login(event) {
      event.preventDefault();
      this.onLogin = true;
      if (!this.email.value.length) {
        this.email.error = true;
      }
      if (!this.password.value.length) {
        this.password.error = true;
      }
      if (this.email.error || this.password.error) return;
      axios
        .post("https://api.quwi.com/v2/auth/login", {
          email: this.email.value,
          password: this.password.value,
        })
        .then((resposnse) => {
          localStorage.setItem("jwt", resposnse.data.token);
          this.$router.push("/home");
        })
        .catch((error) => {
          this.email.error = true;
          this.email.errorText = "Incorrect email or password.";
          this.onLogin = false;
        });
    },
  },
};
</script>

<style lang="css">
.container {
  min-height: calc(100vh - 46px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 46px;
}

.user-login {
  width: 440px;
  margin: auto;
  text-align: center;
}

.b-box {
  padding: 25px 40px 35px;
  background: #fff;
  border: 1px solid rgba(0, 0, 0, 0.11);
  box-shadow: 0 0 12px rgb(0 0 0 / 25%);
  border-radius: 15px;
}

.c-quwi-logo {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 26px;
  color: #000;
  text-decoration: none;
}
.c-quwi-logo img {
  margin-right: 14px;
}

.e-logo-text {
  font-family: Arial, sans-serif;
  font-weight: 700;
  font-size: 24px;
  text-transform: uppercase;
}

.btn {
  height: 52px;
  width: 100%;
  padding: 0 15px;
  font-size: 18px;
  font-weight: 400;
  color: #fff;
  font-family: Arial;
  font-style: normal;
  font-weight: 700;
  background: #2567c0;
  border: none;
  border-radius: 9px;
  cursor: pointer;
  outline: none;
  margin-bottom: 20px;
  margin-top: 17px;
}

.user-login .b-box a:not(.c-quwi-logo) {
  color: #2975dc !important;
  text-decoration: none;
  cursor: pointer;
}
</style>
