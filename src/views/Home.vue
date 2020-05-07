<template>
  <div id="app" style="margin: 0; width: 100vw">
    <h1 style="margin-top: 0; padding-top: 20px;">Get My Password</h1>
    <input
      id="site1"
      type="text"
      placeholder="site"
      class="textField1"
      v-on:keyup.enter="produceEncryption"
      v-model="input.site"
    />
    <input
      id="code1"
      type="password"
      placeholder="password"
      class="textField1"
      v-on:keyup.enter="produceEncryption"
      v-model="input.key"
    />
    <h1 class="err">{{this.errmess}}</h1>
    <h1 class="pass">{{this.res}}</h1>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
const CryptoJS = require("crypto-js");

export default Vue.extend({
  name: "App",
  data() {
    return {
      input: {
        key: "",
        site: ""
      },
      res: "",
      errmess: ""
    };
  },
  methods: {
    produceEncryption() {
      this.res = "";
      var k = this.input.key;
      var s = this.input.site;
      if (k !== "" && s !== "") {
        var full = CryptoJS.HmacSHA256(s, k);
        this.res = this.hex_to_ascii(full);
        this.errmess = "";
      } else {
        this.errmess = "Please enter site or password";
      }
    },
    hex_to_ascii(str1) {
      var hex = str1.toString();
      var str = "";
      for (var n = 0; n < hex.length; n += 2) {
        var strTemp = String.fromCharCode((parseInt(hex.substr(n, 2), 16) % 89) + 33);
        if (str === "/" || str === "\\" || str === "`" || str === "," || str === ".") {
          strTemp = "";
        }
        str += strTemp
      }
      return str;
    }
  }
});
</script>

<style>
#app {
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  margin-top: 60px;
  min-height: 100vh;
  font-size: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.textField1 {
  border: 1px solid #2727271f;
  box-shadow: 1px 1px 5px #27272734;
  border-radius: 50px;
  width: 80vw;
  max-width: 420px;
  height: 10vh;
  max-height: 40px;
  padding: 20px 50px 20px 50px;
  font-size: 35px;
  text-align: center;
  margin: 20px 0px 20px 0px;
}

.pass {
  margin-top: 30px;
  font-size: 25px;
}

.err {
  margin-top: 0px;
  color: rgb(207, 0, 0);
  font-size: 20px;
}

.textField1:enabled {
  outline: 0pt solid #2c3e501a;
}
</style>
