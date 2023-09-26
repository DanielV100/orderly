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
    <div id="dashboard">
        <div id="test">
        <div class="welcome">
            <p id="welcomeMessage"> {{ Config.WELCOMEMESSAGE }}</p> 
            <p id="welcomeName"></p>
            <h2>{{ Config.INTROMESSAGE }} </h2>
        </div>
       
        <div class="wrapper">
            <button class="custom-button">Übung anlegen</button>
            <button class="custom-button">Training zusammenstellen</button>
            <button class="custom-button">Neuen Torwart eintragen</button>
            <button class="custom-button">Ins Dashboard</button>
        </div>
        </div>
    </div>    
</template>



<style scoped>

.wrapper {
    margin-top: 20pt;
    place-self: center;
    min-width: 80%;
    max-width: 80%;
    background: transparent;
    border: 2px solid rgba(255, 255, 255, .2);
    backdrop-filter: blur(20px);
    box-shadow: 0 0 10px rgba(0, 0, 0, .2);
    color: #fff;
    border-radius: 10pt;
    padding: 20pt 20pt 10pt 20pt;
}

#test {
    display: grid;
}

.welcome {
    place-self: center;
    padding: 12pt;
    display: inline-block;
    margin-bottom: auto;
}

#welcomeMessage {
    display: inline;
    font-size: var(--bigHeadingFontSize);
    font-weight: 200;
}
#welcomeName {
    display: inline;
    font-size: var(--bigHeadingFontSize); 
    font-weight: 800;
    color: rgba(100, 200, 0, 1);
}
.custom-button:hover {
    background: rgb(50, 99, 1);
}
.custom-button {
  width: calc(50% - 20pt); 
  margin: 0px 20pt 20pt 0px;
  border: none;
  background-color: var(--green);
  color: #fff;
  font-size: 18pt;
  cursor: pointer;
  border-radius: 5px;
  padding: 20pt;
}
</style>