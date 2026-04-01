<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
<title>UltimateAI - Smart Calculator Hub</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/decimal.js/10.4.3/decimal.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{margin:0;padding:20px;font-family:'Segoe UI',Arial,sans-serif;background:radial-gradient(ellipse at center,#0a0f2a 0%,#03050b 100%);color:white;display:flex;justify-content:center;align-items:center;min-height:100vh;text-align:center;flex-direction:column;position:relative;overflow-x:hidden}
.stars{position:fixed;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:0}
.star{position:absolute;background:white;border-radius:50%;animation:moveUp linear infinite}
@keyframes moveUp{0%{transform:translateY(100vh);opacity:0.8}100%{transform:translateY(-20vh);opacity:0}}
.container{max-width:500px;width:90%;margin:auto;position:relative;z-index:1;background:rgba(10,15,40,0.6);backdrop-filter:blur(10px);border-radius:24px;padding:30px 20px;border:1px solid rgba(255,255,255,0.2);box-shadow:0 10px 40px rgba(0,0,0,0.3)}
.hidden{display:none}
button,.btn,.menu-btn{padding:12px 20px;margin:8px;border:none;border-radius:12px;color:white;background:linear-gradient(135deg,#ff9a9e,#fad0c4);cursor:pointer;transition:all 0.3s ease;font-size:16px;font-weight:600;box-shadow:0 4px 15px rgba(0,0,0,0.2)}
button:hover,.btn:hover,.menu-btn:hover{transform:translateY(-2px);box-shadow:0 6px 20px rgba(0,0,0,0.3);background:linear-gradient(135deg,#ff7a7e,#f8b0a4)}
input,select{padding:12px;width:85%;border-radius:12px;border:1px solid rgba(255,255,255,0.3);text-align:center;margin:10px auto;background:rgba(255,255,255,0.1);color:white;font-size:16px;outline:none;transition:0.2s}
input:focus,select:focus{border-color:#ff9a9e;background:rgba(255,255,255,0.2)}
select option{background:#0a0f2a;color:white}
.result-box{margin:20px auto;padding:15px;background:rgba(0,0,0,0.3);border-radius:16px;border:1px solid rgba(255,255,255,0.15)}
.result,.explain{margin:10px;padding:10px;background:rgba(0,0,0,0.3);border-radius:10px;font-size:16px}
.math-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:15px auto}
.math-grid button{padding:12px;font-size:18px;font-weight:bold}
.physics-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:8px;margin:15px 0;max-height:400px;overflow-y:auto}
.physics-grid button{font-size:12px;padding:10px 8px;margin:4px}
.shape-buttons{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin:15px 0}
.shape-buttons button{font-size:14px;padding:12px}
#puzzleOptions{display:flex;flex-direction:column;gap:10px;margin:20px auto;width:90%}
#puzzleOptions button{width:100%;padding:12px;font-size:16px}
#puzzleQuestion{font-size:20px;font-weight:500;margin:20px;padding:20px;background:rgba(0,0,0,0.4);border-radius:16px}
h1{font-size:28px;background:linear-gradient(135deg,#fff,#ff9a9e);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;margin-bottom:20px}
h2{font-size:24px;margin-bottom:20px;color:#ff9a9e}
.back-btn{margin-top:20px;background:linear-gradient(135deg,#667eea,#764ba2)}
.back-btn:hover{background:linear-gradient(135deg,#5a67d8,#6b46a1)}
.popup-overlay{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:1000}
.popup-content{background:linear-gradient(135deg,#1a1f3a,#0f1328);border:1px solid rgba(255,255,255,0.3);border-radius:20px;padding:30px;width:350px;max-width:90%;text-align:center}
.popup-content input{width:90%;margin:10px auto;padding:10px}
.popup-content button{margin:10px 5px}
.correct-popup{border:2px solid #4caf50;box-shadow:0 0 30px rgba(76,175,80,0.3)}
.wrong-popup{border:2px solid #f44336;box-shadow:0 0 30px rgba(244,67,54,0.3)}
.developer-credit{font-size:12px;color:#aaa;margin-top:20px;padding-top:15px;border-top:1px solid rgba(255,255,255,0.1)}
.developer-credit span{color:#ff9a9e;font-weight:500}
</style>
</head>
<body>

<div class="stars" id="starsContainer"></div>
<div class="container">

<div id="welcome">
    <h1>✨ UltimateAI ✨</h1>
    <p>Advanced calculator with Math, Physics, Puzzles & 3D shapes</p>
    <button id="startBtn">🚀 Start</button>
    <div class="developer-credit">
        Developed by: <span>Mustafa Ahmed Shaheed</span> | <span>Ali Saif</span> | <span>Amir Ghaith Shawqi</span><br>
        With assistance from: <span>Mustafa Ahmed Khurshid</span>
    </div>
</div>

<div id="menu" class="hidden">
    <h2>📱 Select Mode</h2>
    <button class="menu-btn" onclick="showPage('math')">🧮 Math</button>
    <button class="menu-btn" onclick="showPage('physics')">⚡ Physics</button>
    <button class="menu-btn" onclick="showPage('puzzle')">🎮 Puzzle</button>
    <button class="menu-btn" onclick="showPage('ai3d')">🎨 3D & AI</button>
</div>

<div id="math" class="hidden">
    <h2>🧮 Math Calculator</h2>
    <input id="mathInput" placeholder="Enter operation...">
    <div class="math-grid">
        <button onclick="addMath('+')">+</button>
        <button onclick="addMath('-')">-</button>
        <button onclick="addMath('*')">×</button>
        <button onclick="addMath('/')">÷</button>
        <button onclick="addMath('**')">^</button>
        <button onclick="addMath('%')">%</button>
        <button onclick="addMath('Math.sqrt(')">√</button>
        <button onclick="addMath('Math.PI')">π</button>
        <button onclick="addMath('Math.E')">e</button>
    </div>
    <div>
        <button onclick="clearMath()">🗑 Clear</button>
        <button onclick="calculateMath()">= Calculate</button>
    </div>
    <div class="result-box">
        <div class="result" id="mathRes">📊 Result: ---</div>
        <div class="explain" id="mathExplain">📝 Explanation: ---</div>
    </div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

<div id="physics" class="hidden">
    <h2>⚡ Physics Laws (20 Laws)</h2>
    <div class="physics-grid" id="physicsGrid"></div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

<div id="puzzle" class="hidden">
    <h2>🎮 Puzzle Game</h2>
    <select id="puzzleLevelSelect">
        <option value="easy">😊 Easy</option>
        <option value="medium">🤔 Medium</option>
        <option value="hard">🔥 Hard</option>
    </select>
    <button onclick="startPuzzleGame()">🎯 Start Game</button>
    <div id="puzzleQuestion">❓ Select level and press Start</div>
    <div id="puzzleOptions"></div>
    <div id="puzzleScore">⭐ Score: 0</div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

<div id="ai3d" class="hidden">
    <h2>🎨 3D Shapes Calculator</h2>
    <div class="shape-buttons">
        <button onclick="show3DPopup('cube')">📦 Cube</button>
        <button onclick="show3DPopup('sphere')">⚽ Sphere</button>
        <button onclick="show3DPopup('cone')">📐 Cone</button>
        <button onclick="show3DPopup('pyramid')">🔺 Pyramid</button>
        <button onclick="show3DPopup('cylinder')">🧴 Cylinder</button>
        <button onclick="show3DPopup('rectangular prism')">📏 Rectangular Prism</button>
    </div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

</div>

<!-- Physics Popup -->
<div id="physicsPopup" class="popup-overlay">
    <div class="popup-content">
        <h3 id="physicsPopupTitle">Physics Law</h3>
        <div id="physicsPopupFields"></div>
        <div id="physicsPopupResult" style="margin:15px;padding:10px;background:rgba(0,0,0,0.3);border-radius:10px">Result: ---</div>
        <button onclick="closePhysicsPopup()">Close</button>
    </div>
</div>

<!-- 3D Popup -->
<div id="popup3d" class="popup-overlay">
    <div class="popup-content">
        <h3 id="popup3dTitle">3D Shape</h3>
        <div id="popup3dFields"></div>
        <div id="popup3dResult" style="margin:15px;padding:10px;background:rgba(0,0,0,0.3);border-radius:10px">Result: ---</div>
        <button onclick="close3DPopup()">Close</button>
    </div>
</div>

<!-- Puzzle Popup -->
<div id="puzzlePopup" class="popup-overlay">
    <div class="popup-content" id="puzzlePopupContent">
        <div id="puzzlePopupIcon" style="font-size:50px;margin-bottom:10px">✓</div>
        <h3 id="puzzlePopupTitle">Correct!</h3>
        <p id="puzzlePopupMessage">+10 points</p>
        <button onclick="closePuzzlePopup()">Continue</button>
    </div>
</div>

<script>
// Stars
for(let i=0;i<50;i++){
    let s=document.createElement('div');
    s.className='star';
    let size=Math.random()*3+1;
    s.style.width=size+'px';
    s.style.height=size+'px';
    s.style.left=Math.random()*100+'%';
    s.style.animationDuration=(Math.random()*8+5)+'s';
    s.style.animationDelay=(Math.random()*10)+'s';
    document.getElementById('starsContainer').appendChild(s);
}

// Navigation
function showPage(pageId) {
    let pages = ['menu', 'math', 'physics', 'puzzle', 'ai3d'];
    for(let i=0; i<pages.length; i++) {
        let el = document.getElementById(pages[i]);
        if(el) el.classList.add('hidden');
    }
    let target = document.getElementById(pageId);
    if(target) target.classList.remove('hidden');
}

// Start Button
document.getElementById('startBtn').addEventListener('click', function() {
    document.getElementById('welcome').classList.add('hidden');
    document.getElementById('menu').classList.remove('hidden');
});

// ========== MATH FUNCTIONS ==========
function addMath(val){
    let input = document.getElementById('mathInput');
    if(val==='Math.sqrt('){input.value+='Math.sqrt(';}
    else if(val==='Math.PI'){input.value+='Math.PI';}
    else if(val==='Math.E'){input.value+='Math.E';}
    else{input.value+=val;}
}
function clearMath(){
    document.getElementById('mathInput').value='';
    document.getElementById('mathRes').innerHTML='📊 Result: ---';
    document.getElementById('mathExplain').innerHTML='📝 Explanation: ---';
}
function calculateMath(){
    let input = document.getElementById('mathInput').value;
    try{
        let expression = input.replace(/×/g,'*').replace(/÷/g,'/');
        let res = eval(expression);
        document.getElementById('mathRes').innerHTML='📊 Result: '+res;
        document.getElementById('mathExplain').innerHTML='📝 '+input+' = '+res;
    }catch(e){
        document.getElementById('mathRes').innerHTML='❌ Error';
        document.getElementById('mathExplain').innerHTML='⚠️ Invalid expression';
    }
}

// ========== PHYSICS (20 LAWS) ==========
const physicsLaws = [
    { name: "Force", formula: "F = m × a", fields: ['m (kg)', 'a (m/s²)'], calc: (v) => v[0]*v[1], unit: "N" },
    { name: "Pressure", formula: "P = F / A", fields: ['F (N)', 'A (m²)'], calc: (v) => v[0]/v[1], unit: "Pa" },
    { name: "Kinetic Energy", formula: "KE = ½ × m × v²", fields: ['m (kg)', 'v (m/s)'], calc: (v) => 0.5*v[0]*Math.pow(v[1],2), unit: "J" },
    { name: "Potential Energy", formula: "PE = m × g × h", fields: ['m (kg)', 'h (m)'], calc: (v) => v[0]*9.8*v[1], unit: "J" },
    { name: "Work", formula: "W = F × d", fields: ['F (N)', 'd (m)'], calc: (v) => v[0]*v[1], unit: "J" },
    { name: "Power", formula: "P = W / t", fields: ['W (J)', 't (s)'], calc: (v) => v[0]/v[1], unit: "W" },
    { name: "Momentum", formula: "p = m × v", fields: ['m (kg)', 'v (m/s)'], calc: (v) => v[0]*v[1], unit: "kg·m/s" },
    { name: "Density", formula: "ρ = m / V", fields: ['m (kg)', 'V (m³)'], calc: (v) => v[0]/v[1], unit: "kg/m³" },
    { name: "Acceleration", formula: "a = Δv / t", fields: ['Δv (m/s)', 't (s)'], calc: (v) => v[0]/v[1], unit: "m/s²" },
    { name: "Velocity", formula: "v = d / t", fields: ['d (m)', 't (s)'], calc: (v) => v[0]/v[1], unit: "m/s" },
    { name: "Ohm's Law", formula: "V = I × R", fields: ['I (A)', 'R (Ω)'], calc: (v) => v[0]*v[1], unit: "V" },
    { name: "Newton Gravity", formula: "F = G × m₁ × m₂ / r²", fields: ['m₁ (kg)', 'm₂ (kg)', 'r (m)'], calc: (v) => 6.674e-11 * v[0] * v[1] / (v[2]*v[2]), unit: "N" },
    { name: "Hooke's Law", formula: "F = -k × x", fields: ['k (N/m)', 'x (m)'], calc: (v) => v[0]*v[1], unit: "N" },
    { name: "Einstein E=mc²", formula: "E = m × c²", fields: ['m (kg)'], calc: (v) => v[0] * 9e16, unit: "J" },
    { name: "Impulse", formula: "J = F × t", fields: ['F (N)', 't (s)'], calc: (v) => v[0]*v[1], unit: "N·s" },
    { name: "Torque", formula: "τ = F × r", fields: ['F (N)', 'r (m)'], calc: (v) => v[0]*v[1], unit: "N·m" },
    { name: "Frequency", formula: "f = 1 / T", fields: ['T (s)'], calc: (v) => 1/v[0], unit: "Hz" },
    { name: "Wavelength", formula: "λ = v / f", fields: ['v (m/s)', 'f (Hz)'], calc: (v) => v[0]/v[1], unit: "m" },
    { name: "Capacitance", formula: "C = Q / V", fields: ['Q (C)', 'V (V)'], calc: (v) => v[0]/v[1], unit: "F" },
    { name: "Resistance", formula: "R = ρ × L / A", fields: ['ρ (Ω·m)', 'L (m)', 'A (m²)'], calc: (v) => v[0]*v[1]/v[2], unit: "Ω" }
];

let currentPhysicsLaw = null;

function buildPhysicsGrid() {
    const grid = document.getElementById('physicsGrid');
    grid.innerHTML = '';
    physicsLaws.forEach(law => {
        const btn = document.createElement('button');
        btn.textContent = law.name;
        btn.onclick = () => showPhysicsPopup(law);
        grid.appendChild(btn);
    });
}

function showPhysicsPopup(law) {
    currentPhysicsLaw = law;
    document.getElementById('physicsPopupTitle').innerHTML = law.name + ' (' + law.formula + ')';
    let html = '';
    law.fields.forEach((field, idx) => {
        html += `<input type="number" id="phys_${idx}" placeholder="${field}"><br>`;
    });
    html += '<button onclick="calculatePhysics()">Calculate</button>';
    document.getElementById('physicsPopupFields').innerHTML = html;
    document.getElementById('physicsPopupResult').innerHTML = 'Result: ---';
    document.getElementById('physicsPopup').style.display = 'flex';
}

function closePhysicsPopup() {
    document.getElementById('physicsPopup').style.display = 'none';
}

function calculatePhysics() {
    if(!currentPhysicsLaw) return;
    let values = [];
    try {
        for(let i=0; i<currentPhysicsLaw.fields.length; i++) {
            let val = parseFloat(document.getElementById(`phys_${i}`).value);
            if(isNaN(val)) throw new Error();
            values.push(val);
        }
        let result = currentPhysicsLaw.calc(values);
        document.getElementById('physicsPopupResult').innerHTML = `Result: ${result.toFixed(6)} ${currentPhysicsLaw.unit}`;
    } catch(e) {
        document.getElementById('physicsPopupResult').innerHTML = '❌ Error: Enter valid numbers';
    }
}

buildPhysicsGrid();

// ========== 3D POPUP ==========
let current3DShape = '';

function show3DPopup(shape) {
    current3DShape = shape;
    document.getElementById('popup3dTitle').innerHTML = shape.toUpperCase();
    let html = '';
    if(shape === 'cube') {
        html = '<input id="side" type="number" placeholder="Side length (cm)"><br><select id="calcType3D"><option value="volume">Volume (cm³)</option><option value="surface">Surface Area (cm²)</option></select>';
    } else if(shape === 'sphere') {
        html = '<input id="radius" type="number" placeholder="Radius (cm)"><br><select id="calcType3D"><option value="volume">Volume (cm³)</option><option value="surface">Surface Area (cm²)</option></select>';
    } else if(shape === 'cone') {
        html = '<input id="radius" type="number" placeholder="Radius (cm)"><br><input id="height" type="number" placeholder="Height (cm)"><br><select id="calcType3D"><option value="volume">Volume (cm³)</option><option value="surface">Surface Area (cm²)</option></select>';
    } else if(shape === 'pyramid') {
        html = '<input id="base" type="number" placeholder="Base side (cm)"><br><input id="height" type="number" placeholder="Height (cm)"><br><select id="calcType3D"><option value="volume">Volume (cm³)</option><option value="surface">Surface Area (cm²)</option></select>';
    } else if(shape === 'cylinder') {
        html = '<input id="radius" type="number" placeholder="Radius (cm)"><br><input id="height" type="number" placeholder="Height (cm)"><br><select id="calcType3D"><option value="volume">Volume (cm³)</option><option value="surface">Surface Area (cm²)</option></select>';
    } else if(shape === 'rectangular prism') {
        html = '<input id="length" type="number" placeholder="Length (cm)"><br><input id="width" type="number" placeholder="Width (cm)"><br><input id="height" type="number" placeholder="Height (cm)"><br><select id="calcType3D"><option value="volume">Volume (cm³)</option><option value="surface">Surface Area (cm²)</option></select>';
    }
    document.getElementById('popup3dFields').innerHTML = html + '<br><button onclick="calculate3D()">Calculate</button>';
    document.getElementById('popup3dResult').innerHTML = 'Result: ---';
    document.getElementById('popup3d').style.display = 'flex';
}

function close3DPopup() {
    document.getElementById('popup3d').style.display = 'none';
}

function calculate3D() {
    let typeSelect = document.getElementById('calcType3D');
    let type = typeSelect ? typeSelect.value : 'volume';
    let res = 0, unit = '';
    try {
        if(current3DShape === 'cube') {
            let side = parseFloat(document.getElementById('side').value);
            if(type === 'volume') { res = Math.pow(side, 3); unit = 'cm³'; }
            else { res = 6 * Math.pow(side, 2); unit = 'cm²'; }
        } else if(current3DShape === 'sphere') {
            let r = parseFloat(document.getElementById('radius').value);
            if(type === 'volume') { res = (4/3) * Math.PI * Math.pow(r, 3); unit = 'cm³'; }
            else { res = 4 * Math.PI * Math.pow(r, 2); unit = 'cm²'; }
        } else if(current3DShape === 'cone') {
            let r = parseFloat(document.getElementById('radius').value);
            let h = parseFloat(document.getElementById('height').value);
            if(type === 'volume') { res = (1/3) * Math.PI * Math.pow(r, 2) * h; unit = 'cm³'; }
            else { let slant = Math.sqrt(r*r + h*h); res = Math.PI * r * (r + slant); unit = 'cm²'; }
        } else if(current3DShape === 'pyramid') {
            let b = parseFloat(document.getElementById('base').value);
            let h = parseFloat(document.getElementById('height').value);
            if(type === 'volume') { res = (1/3) * Math.pow(b, 2) * h; unit = 'cm³'; }
            else { let slant = Math.sqrt(Math.pow(b/2, 2) + Math.pow(h, 2)); res = Math.pow(b, 2) + 2 * b * slant; unit = 'cm²'; }
        } else if(current3DShape === 'cylinder') {
            let r = parseFloat(document.getElementById('radius').value);
            let h = parseFloat(document.getElementById('height').value);
            if(type === 'volume') { res = Math.PI * Math.pow(r, 2) * h; unit = 'cm³'; }
            else { res = 2 * Math.PI * r * (r + h); unit = 'cm²'; }
        } else if(current3DShape === 'rectangular prism') {
            let l = parseFloat(document.getElementById('length').value);
            let w = parseFloat(document.getElementById('width').value);
            let h = parseFloat(document.getElementById('height').value);
            if(type === 'volume') { res = l * w * h; unit = 'cm³'; }
            else { res = 2 * (l*w + l*h + w*h); unit = 'cm²'; }
        }
        if(isNaN(res)) throw new Error();
        document.getElementById('popup3dResult').innerHTML = 'Result: ' + res.toFixed(4) + ' ' + unit;
    } catch(e) {
        document.getElementById('popup3dResult').innerHTML = '❌ Error: Enter valid numbers';
    }
}

// ========== PUZZLE GAME ==========
let puzzleQuestions = [];
let currentPuzzleIndex = 0;
let puzzleScore = 0;
let pendingNext = false;

function showPuzzlePopup(isCorrect, points, correctAnswer) {
    let popup = document.getElementById('puzzlePopup');
    let content = document.getElementById('puzzlePopupContent');
    let
