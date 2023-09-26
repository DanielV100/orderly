<script setup>
import { ref } from 'vue'; 
import * as Config from './config.js'; 
const usernameInputBackground = ref(); 
const loginButtonDisabled = ref(true); 
const loadingScreenDisabled = ref(true); 
const isPasswordInvsible = ref(true); 
const loadingText = ref(Config.LOADING_1); 

/**
 * Username should only contain a-Z and '.' - this method validates the input. 
 * @param {*} event 
 */
function validateUsername(event) {
    let input = event.target.value; 
    let loginButtonDisable; 
    //if the input is empty default color should be shown
    if(input.length === 0) {
        usernameInputBackground.value = ""; 
        loginButtonDisable = true; 
    } else {
        let lastInputChar = input[input.length-1]; 
        if(Config.VALIDUSERNAMEINPUT.includes(lastInputChar)) {
            usernameInputBackground.value = Config.BG_VALID; 
            loginButtonDisable = false; 
        } else {
            usernameInputBackground.value = Config.BG_INVALID; 
            loginButtonDisable = true; 
        }
    }
    handleLoginButton(loginButtonDisable); 
}

function handleLoginButton(disabled) {
    if(disabled) {
        loginButtonDisabled.value = true; 
    } else {
        loginButtonDisabled.value = false; 
    }
}

/**
 * Function controlls the action which shoulb be triggered when clicking login button. 
 */
function loginButtonClicked() {
  loadingScreenDisabled.value = false; 
  startLoadingAnimation(); 

}

function callDashboard() {
  window.location = '#/dashboard'; 
}

/**
 * Function to show texts during loading. 
 * It's great for the users ux when getting little texts while waiting. 
 */
function startLoadingAnimation() {
  let loadingTextIntervall = setInterval(() => {
    //set second text 
    if(loadingText.value === Config.LOADING_1) {
      loadingText.value = Config.LOADING_2; 
      //set third text 
    } else if(loadingText.value === Config.LOADING_2) {
      loadingText.value = Config.LOADING_3; 
      //call another window
    } else {
      clearInterval(loadingTextIntervall); 
      callDashboard(); 
    }
  }, 2000); 
}

/**
 * Function used for setting the lock opend/closed in password field. 
 */
function tooglePasswordIcon() {
  if(isPasswordInvsible.value === true) {
    setPasswordVisibility("text"); 
    isPasswordInvsible.value = false; 
  } else {
    setPasswordVisibility("password"); 
    isPasswordInvsible.value = true; 
  }
}

/**
 * Getting the password input from DOM and set it visible/unvisible.  
 * @param {'password', 'text'} string  
 */
function setPasswordVisibility(type) {
  document.getElementById('password').setAttribute("type", type); 
}

</script>

<template>
    <div v-if="loadingScreenDisabled" class="wrapper">
        <form action="">
            <h1>{{Config.HEADING}}</h1>
            <div class="input-box">
                <input @input="validateUsername" :style="usernameInputBackground" type="text" placeholder="Username" required>
                <i class='bx bxs-user'></i>
            </div>
            <div class="input-box">
                <input id="password" type="password" placeholder="Password" required>
                <i @click="tooglePasswordIcon" v-if="isPasswordInvsible" class='bx bxs-lock-alt'></i>
                <i @click="tooglePasswordIcon" v-else class='bx bxs-lock-open-alt' ></i>
            </div>
            <button type="submit" class="btn" :disabled="loginButtonDisabled" @click="loginButtonClicked">Login</button>
        </form>
    </div>
    <div class="wrapper" v-else>
      <div class="loader"></div>
      <p class="waittext">{{ loadingText }}</p>
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Nunito+Sans:opsz,wght@6..12,200;6..12,400;6..12,800&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Nunito Sans", sans-serif;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: url('assets/background.jpeg') no-repeat;
  background-size: cover;
  background-position: center;
}

.wrapper {
  width: 620px;
  background: transparent;
  border: 2px solid rgba(255, 255, 255, .2);
  backdrop-filter: blur(20px);
  box-shadow: 0 0 10px rgba(0, 0, 0, .2);
  color: #fff;
  border-radius: 10px;
  padding: 30px 40px;
}

.wrapper h1 {
  font-size: 36px;
  text-align: center;
  font-weight: 800;
}

.wrapper .input-box {
  position: relative;
  width: 100%;
  height: 50px;
  margin: 30px 0;
}

.input-box input {
  width: 100%;
  height: 100%;
  background: transparent;
  border: none;
  outline: none;
  border: 2px solid rgba(255, 255, 255, .2);
  border-radius: 40px;
  font-size: 16px;
  color: #fff;
  padding: 20px 45px 20px 20px;
}

.input-box input::placeholder {
  color: #fff;
}

.input-box i {
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 20px;
}


.wrapper .btn {
  width: 100%;
  height: 45px;
  background: #ffffff;
  border: none;
  outline: none;
  border-radius: 40px;
  box-shadow: 0 0 10px rgba(0, 0, 0, .1);
  cursor: pointer;
  font-size: 16px;
  color: #333;
  font-weight: 600;
}

.wrapper .btn:disabled {
    background: rgba(240, 240, 240, 0.3);
}

.loader {
  margin: auto auto;
  border: 16px solid rgba(100, 200, 0, 0.3);
  border-radius: 50%;
  border-top: 16px solid rgba(100, 200, 0, 1);
  width: 120pt;
  height: 120pt;
  -webkit-animation: spin 2s linear infinite; /* Safari */
  animation: spin 2s linear infinite;
}

/* Safari */
@-webkit-keyframes spin {
  0% { -webkit-transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.waittext {
  text-align: center;
  margin-top: 10px;
  opacity: 0;
  animation: fadeInOut 2s linear infinite;
}

@keyframes fadeInOut {
  0%, 100% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
}
</style>