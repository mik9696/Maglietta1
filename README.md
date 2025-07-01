<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Maglietta Interattiva</title>
<style>
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #8e44ad, #3498db);
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
}
#home {
  width: 100%;
  max-width: 400px;
  padding: 20px;
  text-align: center;
  margin-top: 40px;
}
#home h1 {
  font-size: 2em;
  margin-bottom: 20px;
}
button {
  width: 80%;
  margin: 10px auto;
  padding: 15px;
  font-size: 1.2em;
  border: none;
  border-radius: 20px;
  background: rgba(255,255,255,0.2);
  color: white;
  cursor: pointer;
  backdrop-filter: blur(5px);
  box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  transition: background 0.3s;
}
button:hover {
  background: rgba(255,255,255,0.3);
}
#diceContainer {
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex: 1;
  width: 100%;
  position: relative;
}
#instruction {
  font-size: 1.1em;
  margin-bottom: 10px;
  margin-top: 10px;
}
#scene {
  width: 150px;
  height: 150px;
  perspective: 800px;
  margin-top: 40px;
  margin-bottom: auto;
}
.cube {
  width: 100%;
  height: 100%;
  position: relative;
  transform-style: preserve-3d;
  animation: spin 8s infinite linear;
}
.face {
  position: absolute;
  width: 150px;
  height: 150px;
  background: rgba(255,255,255,0.9);
  color: black;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 0.9em;
  text-align: center;
  border-radius: 10px;
  box-shadow: inset 0 0 20px rgba(0,0,0,0.2);
}
.face1 { transform: rotateY(0deg) translateZ(75px); }
.face2 { transform: rotateY(90deg) translateZ(75px); }
.face3 { transform: rotateY(180deg) translateZ(75px); }
.face4 { transform: rotateY(-90deg) translateZ(75px); }
.face5 { transform: rotateX(90deg) translateZ(75px); }
.face6 { transform: rotateX(-90deg) translateZ(75px); }
#backBtn {
  position: absolute;
  top: 15px;
  left: 15px;
  background: rgba(255,255,255,0.2);
  border: none;
  color: white;
  font-size: 1.2em;
  border-radius: 10px;
  padding: 5px 10px;
  cursor: pointer;
  z-index: 10;
}
#description {
  position: absolute;
  bottom: 20px;
  font-size: 1.2em;
  text-align: center;
  width: 90%;
}
</style>
</head>
<body>
<div id="home">
  <h1>Scegli un tema</h1>
  <button onclick="start('romantico')">Romantico</button>
  <button onclick="start('piccante')">Piccante</button>
  <button onclick="start('simpatico')">Simpatico</button>
</div>
<div id="diceContainer">
  <button id="backBtn" onclick="goHome()">← Indietro</button>
  <div id="instruction">Tocca o swipe il dado per lanciare</div>
  <div id="scene">
    <div class="cube" id="cube">
      <div class="face face1" id="f1"></div>
      <div class="face face2" id="f2"></div>
      <div class="face face3" id="f3"></div>
      <div class="face face4" id="f4"></div>
      <div class="face face5" id="f5"></div>
      <div class="face face6" id="f6"></div>
    </div>
  </div>
  <div id="description"></div>
</div>
<script>
const phrases = {
 romantico: ["Bacio","Balliamo","Aperitivo","Numero","Instagram","Sorpresa"],
 piccante: ["Segreto","Fantasia","Bacio","Più piccante?","Con amica","Trio?"],
 simpatico: ["Bacia chi vuoi","Abbraccia","Verso animale","Complimento","Imita","Faccia buffa"]
};
const descriptions = {
 "Bacio":"Dai un dolce bacio 😘",
 "Balliamo":"Ballate insieme per 1 minuto 🎶",
 "Aperitivo":"Prendete un aperitivo insieme 🍹",
 "Numero":"Scambiatevi il numero 📞",
 "Instagram":"Scambiate Instagram 📱",
 "Sorpresa":"Scegli tu! 😉",
 "Segreto":"Racconta un segreto piccante 🔥",
 "Fantasia":"Di' una tua fantasia 😏",
 "Più piccante?":"La cosa più piccante fatta 🌶️",
 "Con amica":"Bacio con la tua amica 😮",
 "Trio?":"Hai mai fatto una cosa a 3? 😇",
 "Bacia chi vuoi":"Bacia chi vuoi 😘",
 "Abbraccia":"Abbraccia qualcuno 🤗",
 "Verso animale":"Fai il verso di un animale 🐯",
 "Complimento":"Fai un complimento a caso 😊",
 "Imita":"Imita qualcuno 😂",
 "Faccia buffa":"Fai una faccia buffa 😜"
};
let currentTheme = "";
const cube = document.getElementById('cube');
let spinning = true;
let lastFace = 0;

function start(theme) {
 currentTheme = theme;
 document.getElementById('home').style.display = 'none';
 document.getElementById('diceContainer').style.display = 'flex';
 let faces = ['f1','f2','f3','f4','f5','f6'];
 faces.forEach((id,i)=>{
   document.getElementById(id).textContent=phrases[theme][i];
 });
 spinForever();
 document.getElementById('description').textContent="";
}

function goHome(){
 document.getElementById('diceContainer').style.display = 'none';
 document.getElementById('home').style.display = 'block';
}

function spinForever(){
 spinning = true;
 cube.style.transition = "";
 cube.style.animation = "spin 8s infinite linear";
}

function roll(){
 if(!spinning) return;
 spinning=false;
 cube.style.animation = "none";
 let rand = Math.floor(Math.random()*6);
 lastFace = rand;
 let rx=0, ry=0;
 switch(rand){
   case 0: rx=0; ry=0; break;      // face1 front
   case 1: rx=0; ry=-90; break;    // face2
   case 2: rx=0; ry=180; break;    // face3
   case 3: rx=0; ry=90; break;     // face4
   case 4: rx=-90; ry=0; break;    // face5 top
   case 5: rx=90; ry=0; break;     // face6 bottom
 }
 cube.style.transition="transform 2s";
 cube.style.transform=`rotateX(${rx}deg) rotateY(${ry}deg)`;
 setTimeout(()=>{
   let phrase=phrases[currentTheme][rand];
   document.getElementById('description').textContent=descriptions[phrase]||"";
   spinning=true;
 },2000);
}

cube.addEventListener('click', roll);
cube.addEventListener('touchstart', roll);
cube.addEventListener('touchmove', e=>{ e.preventDefault(); roll(); },{passive:false});
</script>
</body>
</html>
