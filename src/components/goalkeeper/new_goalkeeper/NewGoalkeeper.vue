<script setup>
import { onMounted, ref } from "vue";
import * as Config from "./config.js";
const submitButtonDisabled = ref(true);
const sumbmittedSuccessfully = ref(false);
let inputFirstname;
let inputLastname;
let inputClub;
let form; 
onMounted(() => {
  inputFirstname = document.getElementById("firstname");
  inputLastname = document.getElementById("lastname");
  inputClub = document.getElementById("club");
  form = document.getElementById('form'); 
});

function validateMandantoryFieldsFilled(event) {
  if (event.target.value === "") {
    submitButtonDisabled.value = true;
  }
  if (inputFirstname.value && inputClub.value && inputLastname.value) {
    submitButtonDisabled.value = false;
  }
}

function onSubmitClick() {
  sumbmittedSuccessfully.value = true;
  submitButtonDisabled.value = true; 
}

function resetForm() {
  sumbmittedSuccessfully.value = false; 
  form.reset(); 

}
</script>

<template>
  <div id="addgoalie">
    <div class="current_page_indicator">
      <i class="bx bx-home-alt-2"></i>
      <i class="bx bx-chevron-right"></i>
      <p>Neuen Torwart anlegen</p>
    </div>
    <div class="wrapper">
      <form @submit="onSubmitClick" class="form" id="form">
        <div class="column">
          <div class="input-box">
            <label>{{ Config.FORM_FIRSTNAME_LABEL }}*</label>
            <input
              @input="validateMandantoryFieldsFilled"
              id="firstname"
              type="text"
              placeholder="Oliver"
              required
            />
          </div>
          <div class="input-box">
            <label>{{ Config.FORM_LASTNAME_LABEL }}*</label>
            <input
              @input="validateMandantoryFieldsFilled"
              id="lastname"
              type="text"
              placeholder="Kahn"
              required
            />
          </div>
        </div>
        <div class="column">
          <div class="input-box">
            <label>{{ Config.FORM_CLUB }}*</label>
            <input
              @input="validateMandantoryFieldsFilled"
              id="club"
              type="text"
              placeholder="FC Bayern München"
              required
            />
          </div>
          <div class="input-box">
            <label>{{ Config.FORM_BIRTHDAY }}</label>
            <input id="birthday" type="date" />
          </div>
        </div>
        <div class="input-box address">
          <label>{{ Config.FORM_NOTES }}</label>
          <input id="notes" type="text" placeholder="Wir brauchen Eier!" />
        </div>
        <button :disabled="submitButtonDisabled">Torwart anlegen</button>
      </form>
    </div>
    <div v-if="sumbmittedSuccessfully" class="submit-successfull">
      <div class="success-animation">
        <svg class="checkmark" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 52 52">
          <circle class="checkmark__circle" cx="26" cy="26" r="25" fill="none" />
          <path class="checkmark__check" fill="none" d="M14.1 27.2l7.1 7.2 16.7-16.8" />
        </svg>
      </div>
      <div class="success-options">
        <h2>Torwart erfolgreich angelegt! </h2>
        <div class="success-btn">
          <button @click="resetForm" id="btn-new-keeper">Neuen Torwart anlegen </button>
          <button>Ins Menü zurück</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

#btn-new-keeper {
  margin-right: 20px;
}

.success-options {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.success-btn {
  display: flex;
  min-width: 100%;
}

.success-btn button {
  flex: 1;
  color: #fff;
  font-size: 18pt;
  font-weight: 400;
  margin-top: 30px;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
}

.submit-successfull {
  padding-right: 20pt;
  padding-left: 20pt;
  width: 80%;
  display: flex;
  place-self: center;
  align-items: center;
}

.success-animation {
  margin: 20pt auto;
}

.checkmark {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  place-self: center;
  stroke-width: 2;
  stroke: var(--avocadoGreen);
  stroke-miterlimit: 10;
  box-shadow: inset 0px 0px 0px var(--avocadoGreen);
  animation: fill 0.4s ease-in-out 0.4s forwards,scale 0.3s ease-in-out 0.9s both;
}
.checkmark__circle {
  stroke-dasharray: 166;
  stroke-dashoffset: 166;
  stroke-width: 2;
  stroke-miterlimit: 10;
  box-shadow: inset 0px 0px 0px var(--avocadoGreen);
  fill: #fff;
  animation: stroke 0.6s cubic-bezier(0.65, 0, 0.45, 1) forwards;
}

.checkmark__check {
  transform-origin: 50% 50%;
  stroke-dasharray: 48;
  stroke-dashoffset: 48;
  animation: stroke 0.3s cubic-bezier(0.65, 0, 0.45, 1) 0.8s forwards;
}

@keyframes stroke {
  100% {
    stroke-dashoffset: 0;
  }
}

@keyframes scale {
  0%,
  100% {
    transform: none;
  }

  50% {
    transform: scale3d(1.1, 1.1, 1);
  }
}

@keyframes fill {
  100% {
    box-shadow: inset 0px 0px 0px 30px #4bb71b;
  }
}
#addgoalie {
  display: grid;
}
input {
  font-size: 18pt;
}
.wrapper {
  margin-top: 20pt;
  place-self: center;
  min-width: 80%;
  max-width: 80%;
  background: var(--lightGrey);
  border: 2px solid rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(20px);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  color: #fff;
  border-radius: 10pt;
  padding: 20pt 20pt 10pt 20pt;
  text-align: left;
}

.wrapper form input {
  place-content: center;
}

.form .input-box {
  width: 100%;
  margin-top: 20px;
}
.input-box label {
  color: #333;
}
.form :where(.input-box input, .select-box) {
  position: relative;
  height: 50px;
  width: 100%;
  color: #707070;
  margin-top: 8px;
  border: 1px solid #ddd;
  border-radius: 4pt;
  padding: 0 15px;
}
.input-box input:focus {
  box-shadow: 0 1px 0 rgba(0, 0, 0, 0.1);
  color: black;
}
.form .column {
  display: flex;
  column-gap: 15px;
}
.address :where(input, .select-box) {
  margin-top: 15px;
}
.form button {
  height: 55px;
  width: 100%;
  color: #fff;
  font-size: 18pt;
  font-weight: 400;
  margin-top: 30px;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
}

/*Responsive*/
@media screen and (max-width: 500px) {
  .form .column {
    flex-wrap: wrap;
  }
  .form :where(.gender-option, .gender) {
    row-gap: 15px;
  }
}
</style>
