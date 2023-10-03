<script setup>
import { onMounted } from 'vue';

let materialContainer; 
let canvas;
onMounted(() => {
    materialContainer = document.getElementById('materialcontainer'); 
    canvas = document.getElementById('penaltyArea'); 
}); 
function addMaterial(event) {
    const materialContainer = document.getElementById('materialelement');
    const newMaterial = materialContainer.cloneNode(true);
   
    const inputs = newMaterial.querySelectorAll('input');
    inputs.forEach(input => input.value = '');
    

    document.getElementById('materialcontainer').appendChild(newMaterial);

    const ctx = canvas.getContext('2d');

    ctx.translate(canvas.width, 0);
    ctx.rotate(Math.PI / 2);
    ctx.scale(2, 2);
      
      // Outer lines
      ctx.beginPath();
      ctx.rect(0,0, canvas.width, canvas.height);
      ctx.fillStyle = "#060";
      ctx.fill();
      ctx.lineWidth = 1;
      ctx.strokeStyle = "#FFF";
      ctx.stroke();
      ctx.closePath();
      
      ctx.fillStyle = "#FFF";
      
      //Home penalty box
      ctx.beginPath();
      ctx.rect(0, (canvas.height - 322) / 2, 132, 322);
      ctx.stroke();
      ctx.closePath();
      //Home goal box
      ctx.beginPath();
      ctx.rect(0, (canvas.height - 146) / 2, 44, 146);
      ctx.stroke();
      ctx.closePath();
      //Home goal 
      ctx.beginPath();
      ctx.moveTo(1, (canvas.height / 2) - 22);
      ctx.lineTo(1, (canvas.height / 2) + 22);
      ctx.lineWidth = 2;
      ctx.stroke();
      ctx.closePath();
      ctx.lineWidth = 1;

      //Home penalty point
      ctx.beginPath()
      ctx.arc(88, canvas.height / 2, 1, 0, 2*Math.PI, true);
      ctx.fill();
      ctx.closePath();
      //Home half circle
      ctx.beginPath()
      ctx.arc(88, canvas.height / 2, 73, 0.29*Math.PI, 1.71*Math.PI, true);
      ctx.stroke();
      ctx.closePath();
      
     
      //Home L corner
      ctx.beginPath()
      ctx.arc(0, 0, 8, 0, 0.5*Math.PI, false);
      ctx.stroke();
      ctx.closePath();
      //Home R corner
      ctx.beginPath()
      ctx.arc(0, canvas.height, 8, 0, 2*Math.PI, true);
      ctx.stroke();
      ctx.closePath();

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
              placeholder="Oliver"
              required
            />
          </div>
          <div class="input-box">
            <label>Kategorie</label>
            <input list="test" multiple />
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
              placeholder="Kahn"
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
                <input type="number" style="width: 20%" placeholder="0"/>
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
        <p>Metadaten</p>
        <hr />
      </form>
      <div id="test">
        <canvas id="penaltyArea" width="1000" height="500"></canvas>
      </div>
     
    </div>
    
  </div>
</template>

<style scoped>
#test{
    width:20%;
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
input {
  font-size: 18pt;
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
