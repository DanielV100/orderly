<script setup>
import { createTextVNode, onMounted } from "vue";

let materialContainer;
let canvas;
let wrapper;

onMounted(() => {
  materialContainer = document.getElementById("materialcontainer");
  canvas = document.getElementById("penaltyArea");
  wrapper = document.querySelector("#testCanva");
});
function addMaterial(event) {
  const materialContainer = document.getElementById("materialelement");
  const newMaterial = materialContainer.cloneNode(true);

  const inputs = newMaterial.querySelectorAll("input");
  inputs.forEach((input) => (input.value = ""));

  document.getElementById("materialcontainer").appendChild(newMaterial);

  const ctx = canvas.getContext("2d");
  ctx.imageSmoothingEnabled = false;
  ctx.lineWidth = 2; // Experiment with different values
  ctx.lineCap = "round"; // 'butt', 'round', or 'square'

  // Function to draw the football pitch
  function drawPitch() {
    //Outer lines
    ctx.beginPath();
    //needed - otherwise canva is too big  
    canvas.width = wrapper.clientWidth; 
    canvas.height = wrapper.clientHeight; 
    console.log(wrapper.clientWidth); 
    ctx.rect(0, 0, wrapper.clientWidth, canvas.height);
    console.log(wrapper.clientWidth); 
    ctx.fillStyle = "#060";
    ctx.fill();
    ctx.lineWidth = 1;
    ctx.strokeStyle = "#FFF";
    ctx.stroke();
    ctx.closePath();

    //Penalty box
    ctx.beginPath();
    ctx.rect(
      (wrapper.clientWidth / 2) * 0.5045,
      0,
      wrapper.clientWidth / 2,
      (wrapper.clientWidth / 2) * 0.4125
    );
    ctx.stroke();
    ctx.closePath();

    //Goal box
    ctx.beginPath();
    ctx.rect(
      (wrapper.clientWidth / 2) * 0.771,
      0,
      wrapper.clientWidth * 0.229,
      wrapper.clientWidth * 0.06875
    );
    ctx.stroke();
    ctx.closePath();

    //Goal
    ctx.beginPath();
    ctx.moveTo((wrapper.clientWidth / 2) * 0.9085, 2);
    ctx.lineTo((wrapper.clientWidth / 2) * 0.9085 + wrapper.clientWidth * 0.0915, 2);
    ctx.lineWidth = 9;
    ctx.stroke();
    ctx.closePath();

    //Penalty Point
    ctx.beginPath();
    ctx.arc(wrapper.clientWidth / 2, wrapper.clientWidth * 0.1375, 3, 0, 2 * Math.PI, true);
    ctx.fillStyle = "#fff";
    ctx.fill();
    ctx.closePath();

    //Half Circle
    // Half Circle (Opens at the top)
    ctx.beginPath();
    const centerX = wrapper.clientWidth / 2;
    const centerY = (wrapper.clientWidth / 2) * 0.4125; // Adjust the value as needed
    const radius = wrapper.clientWidth * 0.08; // Adjust the value as needed
    const startAngle = 0; // Start from 0 degrees (top)
    const endAngle = Math.PI; // End at 180 degrees (bottom)
    ctx.arc(centerX, centerY, radius, startAngle, endAngle, false);
    ctx.lineWidth = 2;
    ctx.stroke();
    ctx.closePath();

    //Home L corner
    ctx.beginPath();
    ctx.arc(0, 0, 8, 0, 0.5 * Math.PI, false);
    ctx.stroke();
    ctx.closePath();
    //Home R corner
    ctx.beginPath();
    ctx.arc(wrapper.clientWidth, 0, 8, 0, 2 * Math.PI, true);
    ctx.stroke();
    ctx.closePath();
  }
  let isDrawing = false;

  canvas.addEventListener("mousedown", startDrawing);
  canvas.addEventListener("mouseup", stopDrawing);
  canvas.addEventListener("mousemove", draw);

  canvas.addEventListener("touchstart", startDrawing);
  canvas.addEventListener("touchend", stopDrawing);
  canvas.addEventListener("touchmove", draw);

  function startDrawing(e) {
    e.preventDefault();
    const rect = canvas.getBoundingClientRect();
    const x = e.clientX || e.touches[0].clientX;
    const y = e.clientY || e.touches[0].clientY;
    console.log('X: ' + x + 'Y: ' + y);

    isDrawing = true;
    ctx.beginPath();
    ctx.strokeStyle = "#FFF";
    ctx.lineWidth = 3;
    ctx.imageSmoothingEnabled = false;
    ctx.moveTo(x - rect.left, y - rect.top);
  }

  function draw(e) {
    e.preventDefault();
    if (isDrawing) {
      const rect = canvas.getBoundingClientRect();
      console.log(rect); 
      const x = (e.clientX || e.touches[0].clientX) - rect.left;
      const y = (e.clientY || e.touches[0].clientY) - rect.top;
      console.log('X: ' + x + 'Y: ' + y);

      ctx.lineTo(x, y);
      ctx.stroke();
    }
  }

  function stopDrawing() {
    isDrawing = false;
  }
  drawPitch();
}
function methodTest() {
  console.log(canvas.toDataURL()); 
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
                <input type="number" style="width: 20%" placeholder="0" />
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
      <div id="testCanva">
        <canvas id="penaltyArea" width="2000" height="400"></canvas>
      </div>
      <button @click="methodTest">Test</button>
    </div>
  </div>
</template>

<style scoped>
#testCanva {
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
