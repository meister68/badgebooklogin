<template>
  <center>
    <div class="innercont">
      <b-form @submit.prevent="register">
        <b-row>
          <b-col class="signup" sm="4">
            <p class="sign">Sign Up</p>
            <label>
              <p class="label-txt1">ADMIN USERNAME</p>
              <input
                type="text"
                class="input"
                required
                v-model="username"
                
              >
              <div id="unameErr" class="line-box">
                <div class="line"></div>
              </div>
              <transition name="slide-fade">
                <span class="err" v-if="err">Username already taken!</span>
              </transition>
            </label>
            <label>
              <p class="label-txt1">PASSWORD</p>
              <input type="password" class="input" required v-model="password">
              <div class="line-box">
                <div class="line"></div>
              </div>
              <transition name="slide-fade">
                <span
                  class="err"
                  v-show=" password.length < 8 && password  != ''"
                >must be at least 8 characters!</span>
              </transition>
            </label>
            <label>
              <p class="label-txt1">CONFIRM PASSWORD</p>
              <input type="password" class="input" required v-model="confirmpassword">
              <div class="line-box">
                <div class="line"></div>
              </div>
              <transition name="slide-fade">
                <span
                  class="err"
                  v-show="confirmpassword != password && confirmpassword && password"
                >Password doesn't match!</span>
              </transition>
            </label>
          </b-col>
          <b-col id="perinfo" sm="8">
            <p class="sign2">About your organization</p>
            <label>
              <p class="label-txt">NAME OF THE ORGANIZATION</p>
              <input type="text" class="input" required v-model="orgName">
              <div class="line-box">
                <div class="line"></div>
              </div>
            </label>
            <label>
              <p class="label-txt">ADDRESS</p>
              <input type="text" class="input" required v-model="address">
              <div class="line-box">
                <div class="line"></div>
              </div>
            </label>

            <label>
              <p class="label-txt">ADMIN EMAIL ADDRESS</p>
              <input type="email" class="input" required v-model="email">
              <div class="line-box">
                <div class="line"></div>
              </div>
            </label>
            <label>
              <p class="label-txt">
                WHAT YOU DO? (
                Short description)
              </p>
              <input id="description" type="text" class="input" required v-model="description">
              <div class="line-box">
                <div class="line"></div>
              </div>
            </label>

            <button type="submit">submit</button>
          </b-col>
        </b-row>
      </b-form>
    </div>
  </center>
</template>

<script>
import axios from "axios";
import $ from "jquery";

export default {
  name: "OrgSignUp",
  data() {
    return {
      username: "Jonathan",
      password: "jrivas2398",
      confirmpassword: "jrivas2398",
      orgName: "myOrg",
      email: "org@gmail.com",
      address: "asdfasdfasd",
      description: "asdfasdfasdf",
      isValid: true,
      err: false,
      type: ""
    };
  },

  methods: {
    register() {
      let userInfo = {
        username: this.username,
        password: this.password,
        email: this.email,
        address: this.address,
        orgName: this.orgName,
        description: this.description,
        type: "Organization",
        badges: [],
        posts: [],
      };
      if (
        this.isValid &&
        this.password != "" &&
        this.password == this.confirmpassword
      ) {
        this.$store
          .dispatch("orgSignUp", userInfo)
          .then(() => {
            this.err = false;
            $("#unameErr").css({"background-color": "#555657"});
            this.$router.push("/organization")
            
            })
          .catch(err => {
            this.err = true;
            $("#unameErr").css({"background-color": "red"});
          });
      } else {
        alert("invalid credentials!");
      }
    },
    checkUsername() {
      axios
        .post("http://localhost:8081/user/checkusername", {
          username: this.username
        })
        .then(response => {
          this.err = false;
          this.isValid = true;
        })
        .catch(err => {
          this.err = true;
          this.isValid = false;
        });
    },
    logout() {
      this.$store.dispatch("logout").then(() => {
        this.$router.push("/");
      });
    }
  },
  created() {
    // if (
    //   this.username != "" &&
    //   this.password != "" &&
    //   this.confirmpassword != ""
    // ) {
    //   $(".label-txt1").addClass("label-active");
    // }
    // axios.get("http://localhost:8081/user/signedup").then(
    //   response => {
    //     if (response.data.username != null) {
    //       this.username = response.data.username;
    //       this.password = response.data.password;
    //       this.confirmpassword = response.data.password;
    //       $(".label-txt1").addClass("label-active");
    //     }
    //   },
    //   err => {
    //     console.log("error");
    //   }
    // );
  },
  mounted() {
    $(".label-txt")
      .addClass("label-active")
      .css({ color: "#555657" });
    $(".input").focus(function() {
      $(this)
        .parent()
        .find("p")
        .addClass("label-active")
        .css({ color: "#0071ff" });
    });

    $(".input").focusout(function() {
      if ($(this).val() == "") {
        $(this)
          .parent()
          .find("p")
          .removeClass("label-active");
      }
      $(this)
        .parent()
        .find("p")
        .css({ color: "#555657" });
    });
  }
};
</script>


<style scoped>
.slide-fade-enter-active {
  transition: all 0.5s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(10px);
  opacity: 0;
}
.lgt {
  color: red;
  text-decoration: underline;
}

.bcont {
  margin: 0;
  /* background:red; */
}

hr {
  display: block;
  height: 1px;
  border: 0;
  border-top: 1px solid #b3b4b5;
  margin: 1em 0;
  padding: 0;
}
.signup {
  overflow: auto;
  background: #e3e3e3;
  padding: 40px;
  text-align: center;
  border-right: none;
  /* border: 1px solid #bdbebf; */
  height: 648px;
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
  border-right: 1px solid #b3b4b5;
}

#description {
  overflow: visible;
}

.srow {
  margin-left: 0px;
  width: 400px;
}

#years {
  width: 150px;
  padding-left: 20px;
}

#perinfo {
  overflow: auto;
  padding: 40px;
  margin: 0;
  border-radius: 5px;
  height: 100%;
  text-align: left;
}

.gender {
  font-weight: normal;
}
.pholder {
  padding: 0;
}

.rad {
  margin: 16px;
}

.err {
  font-weight: normal;
  color: red;
  font-size: 14px;
  position: absolute;
  left: 0px;
}

.fname {
  width: 100%;
  padding-right: 10px;
  margin: 0;
}
.ln {
  margin-top: 0;
  margin-bottom: 0;
  padding-top: 0;
  padding-bottom: 0;
}

.ln {
  margin-top: 0;
  margin-bottom: 0;
  padding-top: 0;
  padding-bottom: 0;
}

.lname {
  padding-left: 10px;
  width: 100%;
  margin: 0;
}
table {
  width: 100%;
  margin-top: 0;
  padding-top: 0;
  margin: 0;
  padding: 0;
  overflow: visible;
}

.container2 {
  margin-left: 0;
  margin-top: 0;
  padding: 0;
  padding-top: 0;
  width: 100%;
  font-weight: normal;
}

.innercont {
  /* margin-top: 100px; */
  padding: 0;
  border-radius: 2px;
  border: 1px solid #bdbebf;
  overflow: hidden;
  width: 970px;
  height: 650px;
}

.body {
  padding: 0;
  margin: 0;
}

label {
  display: block;
  position: relative;
  margin: 40px 0px;
}
.label-txt {
  position: absolute;
  top: -1.6em;
  font-weight: normal;
  padding: 10px;
  font-family: sans-serif;
  font-size: 1em;
  letter-spacing: 1px;
  color: #555657;
  transition: ease 0.2s;
  margin-top: 15px;
}
.label-txt1 {
  position: absolute;
  top: -1.6em;
  font-weight: normal;
  padding: 10px;
  font-family: sans-serif;
  font-size: 1em;
  letter-spacing: 1px;
  color: #555657;
  transition: ease 0.2s;
  margin-top: 15px;
}

.input {
  width: 100%;
  padding: 10px;
  /* height: 5px; */
  background: transparent;
  border: none;
  outline: none;
  text-align: left;
}

.line-box {
  position: relative;
  width: 100%;
  height: 1px;
  background: #555657;
}

.input {
  font-weight: normal;
}

.line {
  position: absolute;
  width: 0%;
  height: 2px;
  top: 0px;
  transform: translateX(0%);
  background: #0071ff;
  transition: ease 0.6s;
}

.input:focus + .line-box .line {
  width: 100%;
}

.label-active {
  color: #0071ff;
  font-size: 0.8em;
  top: -3em;
}

button {
  padding: 12px 24px;
  background: rgb(220, 220, 220);
  font-weight: bold;
  color: rgb(120, 120, 120);
  border: none;
  outline: none;
  border-radius: 3px;
  cursor: pointer;
  transition: ease 0.3s;
  margin-top: 50px;
  margin-left: 250px;
  /* float:right; */
}

button:hover {
  background: #0071ff;
  color: #ffffff;
}

.sign {
  padding: 0px;
  margin-top: 0;
  font-size: 40px;
  font-weight: unset;
  font-family: sans-serif;
  font-weight: normal;
  margin-bottom: 60px;
}
.sign2 {
  padding: 0px;
  margin-top: 0;
  font-size: 25px;
  font-weight: unset;
  font-family: sans-serif;
  font-weight: normal;
  margin-bottom: 60px;
}
</style>






