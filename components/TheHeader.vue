<template>
  <nav class="app-navbar">
    <a href="/" class="app-quwi"
      ><img width="25px" height="25px" :src="image"
    /></a>
    <div class="b-menu">
      <a href="/" class="" v-if="login"> Signup </a>
      <a href="/" class="" v-if="!login"> Projects </a>
      <a href="/" class="" v-if="!login" @click="logout"> Logout </a>
    </div>
  </nav>
</template>

<script>
import image from "../img/quwi-logo.png";
import axios from "axios";
export default {
  name: "TheHeader",
  props: {
    login: {
      type: Boolean,
    },
  },
  data() {
    return {
      image,
    };
  },
  methods: {
    logout: function (event) {
      event.preventDefault();
      axios
        .post("https://api.quwi.com/v2/auth/logout")
        .then((resposnse) => {
          localStorage.removeItem("jwt");
          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style lang="css">
.app-navbar {
  display: flex;
  background: #fafafa;
  border: none;
  box-shadow: 0 0 5px rgb(0 0 0 / 25%);
  text-align: right;
  padding-right: 20px;
  height: 46px;
  position: fixed;
  width: calc(100% - 20px);
  z-index: 1001;
}
.app-quwi {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28px;
  color: #000;
  font-weight: 700;
  font-family: Google Sans, sans-serif;
  width: 44px;
  height: 44px;
  margin-left: 13px;
  margin-right: 13px;
  margin-top: 1px;
  border-radius: 50%;
}
.b-menu {
  line-height: 46px;
  height: 46px;
  display: inline-flex;
  justify-content: flex-end;
  flex: 1;
}
.b-menu a {
  display: flex;
  float: left;
  align-items: center;
  text-transform: uppercase;
  font-size: 11px;
  color: #636363;
  padding: 0 15px;
  height: 46px;
  white-space: nowrap;
  text-decoration: none;
  cursor: pointer;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
