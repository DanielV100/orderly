<script setup>
import { onMounted } from 'vue';
import * as Config from './config.js'
const username = "Daniel!";
let i = 0; 
 
onMounted(() => {
    typeWriter(); 
}); 
function init() {
    document.body.style.background = "white";  
}

function typeWriter() {
  if (i < username.length) {
    document.getElementById("welcomeName").innerHTML += username.charAt(i);
    i++;
    setTimeout(typeWriter, Config.TYPEWRITTERSPEED);
  }
}

init(); 
</script>

<template>
    <div>
        <div class="welcome">
            <p id="welcomeMessage"> {{ Config.WELCOMEMESSAGE }}</p> 
            <p id="welcomeName"></p>
        </div>
        <div class="wrapper">
            <button class="custom-button">Button 1</button>
            <button class="custom-button">Button 2</button>
            <button class="custom-button">Button 3</button>
        </div>
    </div>
    
</template>

<style scoped>

.welcome {
    display: inline-block;
    margin-bottom: auto;
}

#welcomeMessage {
    display: inline;
    font-size: 32pt;
    font-weight: 200;
}
#welcomeName {
    display: inline;
    font-size: 40px;
    font-weight: 800;
    color: rgba(100, 200, 0, 1);
}

.custom-button {
  width: calc(33.33% - 10px); 
  margin: 0px 10px 10px 0px;
  border: none;
  background-color: #007bff;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  padding: 10px 20px;
}
</style>