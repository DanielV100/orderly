<script setup>
import * as GlobalConfig from '../globals/globalConfig.js'; 

/**
 * What should happen when user wants to log out? 
 * Go to login page and close the session. 
 */
function onClickLogOut() {
    goToLogin(); 
}

/**
 * Changing window location to login screen 
 */
function goToLogin() {
    window.location.href='/'; 
}

</script>

<template>
    <div class="header">
        <a href="#dashboard" class="logo">{{ GlobalConfig.HEADING}}</a>
        <div class="header-right">
            <i class='bx bx-user'></i>
            <i @click="onClickLogOut" class='bx bx-log-out'></i>
        </div>
    </div>
</template>

<style scoped>


* {
    box-sizing: border-box;
}


body {
    display: block;
}
.header {
  overflow: hidden;
  background-color: var(--grey);
}

.header a {
  float: left;
  color: black;
  text-align: center;
  text-decoration: none;
}

.header a.logo {
  padding: 12pt;
  font-size: 25pt;
  font-weight: bold;
}

.header a:hover {
  background-color: #ddd;
  color: black;

}
i {
    padding: 12pt;
    font-size: 35pt;
}
i:hover {
    font-weight: 900;
    cursor: pointer;
    background-color: #ddd;
}
.header-right {
  float: right;
}

@media screen and (max-width: 500px) {
  .header a {
    float: none;
    display: block;
    text-align: left;
  }
  
  .header-right {
    float: none;
  }
}
</style>