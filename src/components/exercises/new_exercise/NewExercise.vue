<script setup>
import { createTextVNode, onMounted, ref } from "vue";
import SoccerField from "../../soccer_field/SoccerField.vue";


const addDrawing = ref(false); 
const addNote = ref(false); 


function addMaterial(event) {
  const materialContainer = document.getElementById("materialelement");
  const newMaterial = materialContainer.cloneNode(true);
  const inputs = newMaterial.querySelectorAll("input");
  inputs.forEach((input) => (input.value = ""));
  document.getElementById("materialcontainer").appendChild(newMaterial);
}

function cloneExistingFields(event, elementContainer, elementToClone) {
  const materialContainer = document.getElementById(elementToClone);
  const newMaterial = materialContainer.cloneNode(true);
  const inputs = newMaterial.querySelectorAll(".input");
  inputs.forEach((input) => (input.value = ""));
  document.getElementById(elementContainer).appendChild(newMaterial);
}

function check(event) {
  addDrawing.value = event.target.checked; 
}

function checkNotes(event) {
  addNote.value = event.target.checked; 
}
</script>

<template>
  <div class="addexcercise">
    <div class="current_page_indicator">
      <i class="bx bx-home-alt-2"></i>
      <i class="bx bx-chevron-right"></i>
      <p>Neue Übung anlegen</p>
    </div>
    <div class="wrapper">
      <form @submit="onSubmitClick" class="form" id="form">
        <div class="column">
          <div class="input-box">
            <label>Titel</label>
            <input
              @input="validateMandantoryFieldsFilled"
              id="firstname"
              type="text"
              placeholder="Koordination"
              required
            />
          </div>
          <div class="input-box">
            <label>Kategorie</label>
            <input 
            list="test" 
            placeholder="Aufwärmen"
            multiple />
            <datalist id="test">
              <option value="Test"></option>
              <option value="Aufwärmen"></option>
              <option value="Abschluss"></option>
            </datalist>
          </div>
          <div class="input-box">
            <label>Anzahl der TW</label>
            <input
              @input="validateMandantoryFieldsFilled"
              id="lastname"
              type="number"
              placeholder="0"
              required
            />
          </div>
        </div>
        <div class="column">
          <div class="input-box">
            <label>Dauer</label>
            <input
              @input="validateMandantoryFieldsFilled"
              id="lastname"
              type="time"
              required
            />
          </div>
          <div class="input-box">
            <label>Intesität</label>
            <input
              @input="validateMandantoryFieldsFilled"
              id="lastname"
              type="number"
              min="0"
              max="5"
              placeholder="0 (gering) - 5 (hoch)"
              required
            />
          </div>
          <div class="input-box" id="multi">
            <div id="materialcontainer">
              <label style="width: 100%">Materialien</label>
              <div id="materialelement" style="display: flex">
                <input class="input" type="number" style="width: 20%; margin-right: 8pt" placeholder="0" />
                <input
                  @input="validateMandantoryFieldsFilled"
                  id="lastname"
                  type="text"
                  style="width: 80%"
                  placeholder="Stangen"
                  list="materials"
                  required
                />
                <datalist id="materials">
                  <option>Stangen</option>
                  <option>Hütchen</option>
                  <option>Bälle</option>
                  <option>Pylonen</option>
                </datalist>
              </div>
            </div>
            <button type="button" @click="addMaterial">+</button>
          </div>
        </div>
        <br />

   
      </form>
      <div id="describtionContainer">
        <form @submit="onSubmitClick" class="form" id="form">
        <div id="describtion">
          <h2>Schritt 1</h2>
          <textarea class="input" id="textTest" rows="3"></textarea>
        </div>
        
      </form>
      </div>
      <button type="button" @click="cloneExistingFields($event, 'describtionContainer', 'describtion')">+</button>
        <br />
       
      
        <br>
      <div>
        <div>
          <input @input="check" type="checkbox" name="testCheck">
          <label for="testCheck">Skizze hinzufügen</label>
        </div>
       
      </div>
      <div v-if="addDrawing" id="soccerFieldContainer">
        <SoccerField />
      </div>
      <br>
      

      <div>
        <div>
          <input @input="checkNotes" type="checkbox" name="testCheck">
          <label for="testCheck">Noziz hinzufügen</label>
        </div>
      </div>
      <div v-if="addNote">
        <h2>Schritt 1</h2>
          <textarea class="input" id="textTest" rows="3"></textarea>
      </div>
    </div>
    
  </div>
</template>

<style scoped>

hr + p {
  font-size: 80pt;
  color: black;
}
#soccerFieldContainer {
  display: grid;
  cursor: crosshair;
  padding: 0;
}
#penaltyArea {
  padding: 0;
}
#multi button {
  margin-top: 10pt;
}
.addexcercise {
  display: grid;
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
  margin-top: 8px;
  padding: 0 15px;
}
.form .column {
  display: flex;
  column-gap: 15px;
}
.address :where(input, .select-box) {
  margin-top: 15px;
}
button {
  height: 55px;
  width: 100%;
  color: #fff;
  font-size: 18pt;
  font-weight: 400;
  margin-top: 5pt;
  transition: all 0.5s ease;
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
