<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Homestead Survival</title>

<style>
*{margin:0;padding:0;box-sizing:border-box}

body{
background:#0a150a;
color:#fff;
font-family:monospace;
overflow:hidden;
}

#game{
width:100vw;
height:100vh;
background:#152015;
position:relative;
overflow:hidden;
}

#world{
position:absolute;
width:1200px;
height:1200px;
background-image:
repeating-linear-gradient(0deg,transparent 31px,#1a2a1a 32px),
repeating-linear-gradient(90deg,transparent 31px,#1a2a1a 32px);
}

.e{
position:absolute;
font-size:28px;
}

#player{z-index:100}
#home{font-size:72px; transition: filter 0.3s ease;}

#moat{
position:absolute;
width:240px;
height:240px;
border-radius:50%;
border:8px solid #2a4a6a;
left:480px;
top:480px;
display:none;
opacity:0.7;
box-shadow: inset 0 0 20px rgba(42,74,106,0.5);
z-index:5;
}

#safeZone{
position:absolute;
width:220px;
height:220px;
border-radius:50%;
border:3px dashed rgba(68,255,136,0.4);
left:490px;
top:490px;
display:none;
z-index:6;
pointer-events:none;
box-shadow: 0 0 30px rgba(68,255,136,0.2), inset 0 0 30px rgba(68,255,136,0.1);
animation: pulseSafeZone 2s ease-in-out infinite;
}

@keyframes pulseSafeZone{
0%, 100% { opacity: 0.3; transform: scale(1); }
50% { opacity: 0.6; transform: scale(1.02); }
}

.outpostZone{
position:absolute;
width:160px;
height:160px;
border-radius:50%;
border:2px dashed rgba(255,204,68,0.5);
z-index:6;
pointer-events:none;
box-shadow: 0 0 20px rgba(255,204,68,0.2);
animation: pulseSafeZone 2s ease-in-out infinite;
}

.den{
position:absolute;
font-size:48px;
z-index:15;
filter: drop-shadow(0 0 8px rgba(255,0,0,0.4));
}

.den.cleared{
filter: drop-shadow(0 0 8px rgba(68,255,136,0.6));
opacity:0.9;
}

#chest{
position:absolute;
font-size:48px;
left:480px;
top:620px;
display:none;
z-index:10;
}

/* ARROW */
#arrow{
position:absolute;
font-size:26px;
color:#4f8;
transform-origin:center;
pointer-events:none;
}

#ui{
position:fixed;
top:10px;
left:10px;
background:rgba(0,0,0,.7);
padding:10px;
border-radius:10px;
z-index:999;
line-height:1.6;
}

#timeUI{
border-bottom:1px solid #355;
padding-bottom:6px;
margin-bottom:6px;
font-size:14px;
}

#outpostUI{
border-top:1px solid #355;
padding-top:6px;
margin-top:6px;
font-size:12px;
color:#fc4;
}

.btn{
position:fixed;
bottom:20px;
left:20px;
width:75px;
height:75px;
border-radius:18px;
border:3px solid #4f8;
background:#1a3a1a;
color:white;
font-size:30px;
z-index:999;
}

#attackBtn{
position:fixed;
bottom:110px;
left:20px;
width:75px;
height:75px;
border-radius:18px;
border:3px solid #f44;
background:#3a1a1a;
color:white;
font-size:30px;
z-index:999;
}

#joystick{
position:fixed;
bottom:20px;
right:20px;
width:140px;
height:140px;
border-radius:50%;
background:rgba(255,255,255,.08);
border:3px solid #4f8;
z-index:999;
touch-action:none;
}

#stick{
position:absolute;
left:45px;
top:45px;
width:50px;
height:50px;
border-radius:50%;
background:#4f8;
}

/* MINIMAP */
#minimap{
position:fixed;
top:10px;
right:10px;
width:160px;
height:160px;
background:rgba(0,0,0,0.85);
border:2px solid #4f8;
border-radius:8px;
z-index:998;
overflow:hidden;
}

#minimapCanvas{
width:100%;
height:100%;
}

/* FOG OF WAR */
#fogCanvas{
position:absolute;
top:0;
left:0;
width:1200px;
height:1200px;
pointer-events:none;
z-index:20;
}

/* DAY/NIGHT SYSTEM */
#dayNightOverlay{
position:fixed;
top:0;
left:0;
width:100vw;
height:100vh;
pointer-events:none;
z-index:500;
background:rgba(5,10,25,0.85);
opacity:0;
transition: opacity 2s ease;
}

#dayNightOverlay.night{
opacity:1;
}

#visionMask{
position:fixed;
top:0;
left:0;
width:100vw;
height:100vh;
pointer-events:none;
z-index:501;
opacity:0;
transition: opacity 2s ease;
}

#visionMask.night{
opacity:1;
background:radial-gradient(circle 200px at center, transparent 0%, rgba(5,10,25,0.95) 100%);
}

#sunMoonTrack{
position:fixed;
top:40px;
left:10%;
right:10%;
height:4px;
background:rgba(255,255,255,0.2);
border-radius:2px;
z-index:998;
}

#sunMoon{
position:absolute;
top:-12px;
font-size:28px;
transition: left 0.5s linear;
filter:drop-shadow(0 0 8px rgba(255,255,255,0.5));
}

/* MENU */
#homeMenu{
position:fixed;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:rgba(0,0,0,.95);
width:320px;
max-height:80vh;
border-radius:12px;
border:2px solid #4f8;
z-index:2000;
display:none;
flex-direction:column;
padding:12px;
overflow:hidden;
}

#menuTabs{
display:flex;
gap:8px;
margin-bottom:12px;
border-bottom:2px solid #355;
padding-bottom:8px;
}

.tabBtn{
flex:1;
padding:8px;
background:#132313;
border:1px solid #355;
color:#aaa;
border-radius:6px;
cursor:pointer;
font-family:monospace;
font-size:13px;
transition: all 0.2s ease;
}

.tabBtn.active{
background:#1a3a1a;
border-color:#4f8;
color:#fff;
}

.tabContent{
display:none;
overflow-y:auto;
flex:1;
}

.tabContent.active{
display:block;
}

#craftList{overflow-y:auto;flex:1}

.craftItem{
background:#132313;
border:1px solid #355;
padding:8px;
border-radius:8px;
margin-bottom:6px;
transition: all 0.2s ease;
}

.craftItem.completed{
background:#1a3a1a;
border-color:#4f8;
box-shadow: 0 0 8px rgba(68,255,136,0.3);
}

.craftItem.completed .itemLabel::after{
content:' ✅';
}

.craftItem button{
width:100%;
margin-top:6px;
padding:8px;
background:#1a3a1a;
border:2px solid #4f8;
color:white;
border-radius:6px;
cursor:pointer;
transition: all 0.2s ease;
font-family:monospace;
}

.craftItem button:hover:not(:disabled){
background:#2a4a2a;
}

.craftItem button:disabled{
background:#1a1a1a;
border-color:#355;
color:#666;
cursor:not-allowed;
}

.craftItem.completed button{
background:#2a4a2a;
border-color:#6fa;
}

.colorOptions{
display:flex;
gap:6px;
margin-top:6px;
flex-wrap:wrap;
}

.colorBtn{
width:36px;
height:36px;
border-radius:6px;
border:2px solid #355;
cursor:pointer;
font-size:20px;
display:flex;
align-items:center;
justify-content:center;
transition: all 0.2s ease;
}

.colorBtn:hover{
border-color:#4f8;
transform:scale(1.1);
}

.colorBtn.selected{
border-color:#4f8;
box-shadow: 0 0 8px rgba(68,255,136,0.5);
}

.exitBtn{
width:100%;
margin-top:8px;
padding:10px;
background:#3a1a1a;
border:2px solid #f44;
color:white;
border-radius:6px;
cursor:pointer;
font-family:monospace;
}

#repairProgress{
margin-top:6px;
padding-top:6px;
border-top:1px solid #355;
font-size:14px;
}

#customTitle{
font-size:16px;
text-align:center;
margin-bottom:8px;
color:#4f8;
}

#healToast{
position:fixed;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:rgba(20,60,20,.95);
border:2px solid #4f8;
border-radius:12px;
padding:16px 24px;
z-index:2500;
display:none;
font-size:18px;
color:#4f8;
box-shadow: 0 0 20px rgba(68,255,136,0.6);
animation: fadeInOut 1.5s ease;
}

#captureToast{
position:fixed;
top:50%;
left:50%;
transform:translate(-50%,-50%);
background:rgba(255,204,68,.95);
border:2px solid #fc4;
border-radius:12px;
padding:16px 24px;
z-index:2500;
display:none;
font-size:18px;
color:#fff;
box-shadow: 0 0 20px rgba(255,204,68,0.6);
animation: fadeInOut 2s ease;
}

.damageIndicator{
position:absolute;
font-size:20px;
font-weight:bold;
color:#f44;
z-index:200;
pointer-events:none;
animation: floatUp 0.8s ease-out forwards;
}

@keyframes floatUp{
0%{opacity:1;transform:translateY(0);}
100%{opacity:0;transform:translateY(-40px);}
}

@keyframes fadeInOut{
0% { opacity:0; transform:translate(-50%,-50%) scale(0.8); }
20% { opacity:1; transform:translate(-50%,-50%) scale(1.05); }
25% { transform:translate(-50%,-50%) scale(1); }
80% { opacity:1; }
100% { opacity:0; }
}

/* STARTUP INSTRUCTIONS OVERLAY */
#instructionsOverlay{
position:fixed;
top:0;
left:0;
width:100vw;
height:100vh;
background:rgba(0,0,0,0.9);
z-index:3000;
display:flex;
align-items:center;
justify-content:center;
backdrop-filter:blur(4px);
padding:20px;
overflow-y:auto;
}

#instructionsBox{
background:#132313;
border:3px solid #4f8;
border-radius:16px;
padding:24px;
max-width:420px;
width:100%;
max-height:calc(100vh - 180px);
overflow-y:auto;
text-align:center;
box-shadow: 0 0 30px rgba(68,255,136,0.3);
margin:auto;
}

#instructionsBox h2{
color:#4f8;
margin-bottom:16px;
font-size:24px;
}

#instructionsBox ul{
text-align:left;
margin:16px 0;
padding-left:20px;
line-height:1.8;
font-size:13px;
}

#instructionsBox li{
margin-bottom:8px;
}

#startGameBtn{
width:100%;
padding:16px;
background:#1a3a1a;
border:3px solid #4f8;
color:white;
border-radius:8px;
cursor:pointer;
font-family:monospace;
font-size:18px;
margin-top:16px;
margin-bottom:8px;
transition: all 0.2s ease;
font-weight:bold;
box-shadow: 0 4px 12px rgba(68,255,136,0.4);
position:relative;
z-index:3001;
}

#startGameBtn:hover{
background:#2a4a2a;
transform:scale(1.02);
box-shadow: 0 6px 16px rgba(68,255,136,0.6);
}

#startGameBtn:active{
transform:scale(0.98);
}

#starvedOverlay{
position:fixed;
top:0;
left:0;
width:100vw;
height:100vh;
pointer-events:none;
z-index:1500;
box-shadow: inset 0 0 200px rgba(200,0,0,0.6);
display:none;
}

.denHealthBar{
position:absolute;
width:60px;
height:6px;
background:rgba(0,0,0,0.7);
border:1px solid #f44;
border-radius:3px;
z-index:16;
display:none;
}

.denHealthFill{
height:100%;
background:#4f8;
border-radius:2px;
transition: width 0.2s ease;
}

/* GAME OVER SCREEN */
#gameOverOverlay{
position:fixed;
top:0;
left:0;
width:100vw;
height:100vh;
background:rgba(0,0,0,0.95);
z-index:4000;
display:none;
align-items:center;
justify-content:center;
backdrop-filter:blur(6px);
}

#gameOverBox{
background:#231313;
border:3px solid #f44;
border-radius:16px;
padding:32px;
max-width:400px;
width:90%;
text-align:center;
box-shadow: 0 0 40px rgba(255,68,68,0.5);
}

#gameOverBox h2{
color:#f44;
margin-bottom:16px;
font-size:32px;
text-shadow: 0 0 10px rgba(255,68,68,0.8);
}

#gameOverStats{
margin:20px 0;
padding:16px;
background:rgba(0,0,0,0.5);
border-radius:8px;
line-height:1.8;
}

#restartBtn{
width:100%;
padding:14px;
background:#3a1a1a;
border:3px solid #f44;
color:white;
border-radius:8px;
cursor:pointer;
font-family:monospace;
font-size:16px;
margin-top:12px;
transition: all 0.2s ease;
}

#restartBtn:hover{
background:#4a2a2a;
transform:scale(1.02);
}

.wolf{
z-index:50;
transition: transform 0.1s ease;
position:relative;
}

.wolfState{
position:absolute;
top:-20px;
left:50%;
transform:translateX(-50%);
font-size:14px;
z-index:51;
text-shadow: 0 0 4px rgba(0,0,0,0.8);
pointer-events:none;
}

.wolf.aggressive{
filter:drop-shadow(0 0 6px rgba(255,0,0,0.8));
}

.wolf.passive{
filter:drop-shadow(0 0 4px rgba(100,100,100,0.4));
}

/* MOBILE OPTIMIZATION */
@media (max-width: 480px){
#instructionsBox{
padding:20px 16px;
max-height:calc(100vh - 220px);
}

#instructionsBox h2{
font-size:20px;
margin-bottom:12px;
}

#instructionsBox ul{
font-size:12px;
margin:12px 0;
}

#startGameBtn{
padding:14px;
font-size:16px;
}

#minimap{
width:120px;
height:120px;
}
}
</style>
</head>

<body>

<div id="instructionsOverlay">
<div id="instructionsBox">
<h2>🏚️ Homestead Survival</h2>
<p>Defend your castle & conquer the wilderness!</p>
<ul>
<li>🎮 Move with joystick (bottom right)</li>
<li>✋ Tap hand to gather/craft</li>
<li>⚔️ Tap sword to attack wolves (8 dmg)</li>
<li>🌳 Collect 🪵 wood from trees</li>
<li>🪨 Mine ⚙️ metal from rocks</li>
<li>🍖 Find food on the ground</li>
<li>🍳 Cooked food: +30 hunger & +7 health</li>
<li>🏠 Get close to home to repair it</li>
<li>🏰 Upgrade to level 5 castle!</li>
<li>🏚️ <b style="color:#f44;">WOLF DENS:</b> 3 camps guard territory day & night</li>
<li>☀️ Day: Wolves passive if you stay >60px from them & >100px from den</li>
<li>🌙 Night: Wolves aggressive - chase in 150px radius!</li>
<li>😴 Passive wolves show "zzz", aggressive show red eyes</li>
<li>⚔️ Defeat all wolves to capture dens</li>
<li>💰 Each cleared den: +50🪵 +20🍖</li>
<li>🛡️ Captured dens = 80px safe zones</li>
<li>☁️ Fog hides unexplored areas</li>
<li>☀️🌙 2 min cycle: 80s day, 40s night</li>
<li>🌊 Moat expands safe zone to 120px</li>
</ul>
<p style="margin-top:12px;color:#ff8;font-size:13px;">⚠️ Hunger drops 3 pts per 20s (6 at night)</p>
<p style="color:#f88;font-size:13px;">⚠️ Stay back during day, prepare for night!</p>
<button id="startGameBtn" onclick="startGame()">Start Game</button>
</div>
</div>

<div id="dayNightOverlay"></div>
<div id="visionMask"></div>
<div id="starvedOverlay"></div>
<div id="healToast">+30 Hunger, +7 Health</div>
<div id="captureToast">Den Captured! +50🪵 +20🍖</div>

<div id="gameOverOverlay">
<div id="gameOverBox">
<h2>💀 GAME OVER</h2>
<p>The wolves got you...</p>
<div id="gameOverStats">
<p>🏠 Reached Level: <span id="finalLevel">1</span></p>
<p>⏱️ Survived: <span id="finalTime">0:00</span></p>
<p>🐺 Wolves Defeated: <span id="finalKills">0</span></p>
<p>🏚️ Dens Captured: <span id="finalDens">0</span></p>
</div>
<button id="restartBtn" onclick="location.reload()">Try Again</button>
</div>
</div>

<div id="minimap">
<canvas id="minimapCanvas"></canvas>
</div>

<div id="sunMoonTrack">
<div id="sunMoon">☀️</div>
</div>

<div id="game">
<div id="world">
<canvas id="fogCanvas"></canvas>

<div id="safeZone"></div>
<div id="moat"></div>
<div class="e" id="chest">💰</div>
<div class="e" id="home" style="left:560px;top:560px;">🏚️</div>

<div class="e" id="player" style="left:600px;top:600px;">
🧍
<div id="arrow">➤</div>
</div>

</div>
</div>

<div id="ui">
<div id="timeUI">☀️ Day: <span id="timeDisplay">1:20</span></div>
❤️ <span id="health">100</span>/100<br>
🪵 <span id="wood">0</span><br>
⚙️ <span id="metal">0</span><br>
🍖 <span id="food">5</span><br>
😋 Hunger: <span id="hunger">100</span>/100<br>
🏠 Level: <span id="level">1</span>
<div id="repairProgress">Repairs: 🪟❌ 🧱❌ 🚪❌ 🪵❌</div>
<div id="outpostUI">🏚️ Dens: 0/3 Cleared</div>
</div>

<button class="btn" id="act">✋</button>
<button id="attackBtn">⚔️</button>

<div id="joystick"><div id="stick"></div></div>

<div id="homeMenu">

<div id="menuTabs">
<button class="tabBtn active" onclick="switchTab('repairs')">Repairs</button>
<button class="tabBtn" id="customTabBtn" style="display:none;" onclick="switchTab('custom')">Custom</button>
</div>

<div id="repairTab" class="tabContent active">
<div class="craftItem" id="item-cook">
<span class="itemLabel">🍖 Cook Food (1🍖)</span>
<span style="font-size:11px;display:block;color:#aaa;margin-top:4px;">Restore 30 hunger & 7 health</span>
<button onclick="cookFood()">Cook</button>
</div>

<div class="craftItem" id="item-window">
<span class="itemLabel">🪟 Window (2🪵)</span>
<button onclick="repair('window')">Repair</button>
</div>

<div class="craftItem" id="item-wall">
<span class="itemLabel">🧱 Wall (2🪵 1⚙️)</span>
<button onclick="repair('wall')">Repair</button>
</div>

<div class="craftItem" id="item-door">
<span class="itemLabel">🚪 Door (1🪵 2⚙️)</span>
<button onclick="repair('door')">Repair</button>
</div>

<div class="craftItem" id="item-roof">
<span class="itemLabel">🪵 Roof (3🪵 2⚙️)</span>
<button onclick="repair('roof')">Repair</button>
</div>
</div>

<div id="customTab" class="tabContent">
<div id="customTitle">🏰 Castle Customizations</div>

<div class="craftItem" id="item-paint">
<span class="itemLabel">🎨 Paint House</span>
<div class="colorOptions" id="paintOptions">
<div class="colorBtn" data-color="none" onclick="paintHouse('none')" style="background:#fff;">⚪</div>
<div class="colorBtn" data-color="red" onclick="paintHouse('red')" style="background:#f44;">🔴</div>
<div class="colorBtn" data-color="blue" onclick="paintHouse('blue')" style="background:#44f;">🔵</div>
<div class="colorBtn" data-color="green" onclick="paintHouse('green')" style="background:#4f4;">🟢</div>
<div class="colorBtn" data-color="yellow" onclick="paintHouse('yellow')" style="background:#ff4;">🟡</div>
<div class="colorBtn" data-color="purple" onclick="paintHouse('purple')" style="background:#94f;">🟣</div>
</div>
</div>

<div class="craftItem" id="item-sledgehammer">
<span class="itemLabel">🔨 Sledgehammer (8🪵 5⚙️)</span>
<span style="font-size:11px;display:block;color:#aaa;margin-top:4px;">+1 resource per gather</span>
<button onclick="craftUpgrade('sledgehammer')">Craft</button>
</div>

<div class="craftItem" id="item-garden">
<span class="itemLabel">🌻 Garden (4🪵 2🍖)</span>
<div class="colorOptions" id="gardenOptions">
<div class="colorBtn" onclick="craftGarden('pink')">🌸</div>
<div class="colorBtn" onclick="craftGarden('yellow')">🌻</div>
<div class="colorBtn" onclick="craftGarden('red')">🌹</div>
<div class="colorBtn" onclick="craftGarden('purple')">🌷</div>
<div class="colorBtn" onclick="craftGarden('white')">🌼</div>
</div>
<button id="gardenBtn" onclick="craftGarden()" style="display:none;">Plant Garden</button>
</div>

<div class="craftItem" id="item-chest">
<span class="itemLabel">💰 Treasure Chest (10🪵 6⚙️)</span>
<span style="font-size:11px;display:block;color:#aaa;margin-top:4px;">Fits the castle aesthetic better. Store your extra resources</span>
<button onclick="craftUpgrade('chest')">Craft</button>
</div>

<div class="craftItem" id="item-moat">
<span class="itemLabel">🌊 Moat (12🪵 8⚙️)</span>
<span style="font-size:11px;display:block;color:#aaa;margin-top:4px;">Defensive water ring - expands safe zone to 120px!</span>
<button onclick="craftUpgrade('moat')">Craft</button>
</div>
</div>

<button class="exitBtn" onclick="closeMenu()">Exit</button>

</div>

<script>

const world=document.getElementById('world')
const player=document.getElementById('player')
const arrow=document.getElementById('arrow')
const home=document.getElementById('home')
const safeZone=document.getElementById('safeZone')
const dayNightOverlay=document.getElementById('dayNightOverlay')
const visionMask=document.getElementById('visionMask')
const sunMoon=document.getElementById('sunMoon')
const healToast=document.getElementById('healToast')
const captureToast=document.getElementById('captureToast')
const minimapCanvas=document.getElementById('minimapCanvas')
const fogCanvas=document.getElementById('fogCanvas')
const mCtx=minimapCanvas.getContext('2d')
const fCtx=fogCanvas.getContext('2d')

// World dimensions
const WORLD_SIZE=1200
fogCanvas.width=WORLD_SIZE
fogCanvas.height=WORLD_SIZE
minimapCanvas.width=160
minimapCanvas.height=160

let x=600,y=600
const homeX=560,homeY=560

let wood=0,metal=0,food=5,hunger=100,health=100
let homeLevel=1

// DAY/NIGHT SYSTEM
const CYCLE_LENGTH=120000
const DAY_LENGTH=80000
const NIGHT_LENGTH=40000
let cycleStartTime=Date.now()
let isNight=false
let gameStarted=false
let gameStartTime=Date.now()
let gameOver=false

// WOLF SYSTEM - Updated with day/night AI
let wolves=[]
let wolvesKilled=0
const WOLF_SPEED=1.2
const WOLF_DAMAGE=15
const WOLF_HEALTH=20
const WOLF_ATTACK_DAMAGE=8
const HOME_SAFE_RADIUS=110
const MOAT_SAFE_RADIUS=120
const WOLF_PATROL_RADIUS=150
const OUTPOST_SAFE_RADIUS=80
const WOLF_DAY_AGGRO_DIST=60
const WOLF_DAY_DEN_AGGRO_DIST=100
const WOLF_NIGHT_AGGRO_DIST=150
const WOLF_DEAGGRO_TIME=3000

// DEN SYSTEM
let dens=[]
let densCleared=0
const DEN_POSITIONS=[
  {x:200,y:200},{x:1000,y:200},{x:700,y:900}
]

// FOG OF WAR
let exploredTiles=new Set()
const TILE_SIZE=40
const VIEW_RADIUS=200

// Customization state
let hasSledgehammer=false
let hasChest=false
let hasMoat=false
let gardenColor=null
let houseColor='none'
let gardenElements=[]

let repairProgress={
window:0,
wall:0,
door:0,
roof:0
}

let entities=[]

// Initialize fog of war
function initFog(){
  fCtx.fillStyle='rgba(0,0,0,0.92)'
  fCtx.fillRect(0,0,WORLD_SIZE,WORLD_SIZE)
}

initFog()

/* SPAWN SYSTEM */
function spawn(type,emoji,px,py){
const e=document.createElement('div')
e.className='e'
e.textContent=emoji
e.style.left=px+'px'
e.style.top=py+'px'
world.appendChild(e)
entities.push({type,x:px,y:py,el:e,hp:3})
}

// Populate world
for(let i=0;i<50;i++)spawn('tree','🌳',Math.random()*1100,Math.random()*1100)
for(let i=0;i<25;i++)spawn('rock','🪨',Math.random()*1100,Math.random()*1100)
for(let i=0;i<15;i++)spawn('food','🍖',Math.random()*1100,Math.random()*1100)

// Spawn wolf dens
function spawnDens(){
  DEN_POSITIONS.forEach((pos,i)=>{
    const den=document.createElement('div')
    den.className='e den'
    den.textContent='🏚️'
    den.style.left=pos.x+'px'
    den.style.top=pos.y+'px'
    world.appendChild(den)
    
    const healthBar=document.createElement('div')
    healthBar.className='denHealthBar'
    healthBar.style.left=(pos.x-30)+'px'
    healthBar.style.top=(pos.y-15)+'px'
    const fill=document.createElement('div')
    fill.className='denHealthFill'
    fill.style.width='100%'
    healthBar.appendChild(fill)
    world.appendChild(healthBar)
    
    dens.push({
      x:pos.x,
      y:pos.y,
      el:den,
      healthBar:healthBar,
      healthFill:fill,
      wolves:[],
      maxWolves:2,
      cleared:false,
      id:i
    })
  })
}

spawnDens()

/* SPAWN WOLVES AT DENS ON START */
function spawnInitialWolves(){
  dens.forEach(den=>{
    for(let i=0;i<den.maxWolves;i++){
      spawnWolfAtDen(den)
    }
  })
}

function spawnWolfAtDen(den){
if(den.cleared||den.wolves.length>=den.maxWolves) return

const angle=Math.random()*Math.PI*2
const dist=60+Math.random()*40
const wx=den.x+Math.cos(angle)*dist
const wy=den.y+Math.sin(angle)*dist

const wolfEl=document.createElement('div')
wolfEl.className='e wolf passive'
wolfEl.textContent='🐺'
wolfEl.style.left=wx+'px'
wolfEl.style.top=wy+'px'

const stateEl=document.createElement('div')
stateEl.className='wolfState'
stateEl.textContent='💤'
wolfEl.appendChild(stateEl)

world.appendChild(wolfEl)

const wolf={
  x:wx,
  y:wy,
  el:wolfEl,
  stateEl:stateEl,
  hp:WOLF_HEALTH,
  attackCooldown:0,
  denId:den.id,
  denX:den.x,
  denY:den.y,
  aggressive:false,
  deaggroTimer:0,
  patrolAngle:Math.random()*Math.PI*2,
  patrolSpeed:0.5
}

wolves.push(wolf)
den.wolves.push(wolf)
}

function updateFog(){
  const gridX=Math.floor(x/TILE_SIZE)
  const gridY=Math.floor(y/TILE_SIZE)
  const viewTiles=Math.ceil(VIEW_RADIUS/TILE_SIZE)
  
  for(let dx=-viewTiles;dx<=viewTiles;dx++){
    for(let dy=-viewTiles;dy<=viewTiles;dy++){
      const tx=gridX+dx
      const ty=gridY+dy
      if(tx>=0&&tx<WORLD_SIZE/TILE_SIZE&&ty>=0&&ty<WORLD_SIZE/TILE_SIZE){
        const dist=Math.hypot(dx,dy)
        if(dist<=viewTiles){
          exploredTiles.add(`${tx},${ty}`)
        }
      }
    }
  }
  
  fCtx.clearRect(0,0,WORLD_SIZE,WORLD_SIZE)
  fCtx.fillStyle='rgba(0,0,0,0.92)'
  fCtx.fillRect(0,0,WORLD_SIZE,WORLD_SIZE)
  
  exploredTiles.forEach(key=>{
    const [tx,ty]=key.split(',').map(Number)
    fCtx.clearRect(tx*TILE_SIZE,ty*TILE_SIZE,TILE_SIZE,TILE_SIZE)
  })
  
  dens.forEach(den=>{
    if(den.cleared){
      fCtx.save()
      fCtx.globalCompositeOperation='destination-out'
      fCtx.beginPath()
      fCtx.arc(den.x,den.y,OUTPOST_SAFE_RADIUS,0,Math.PI*2)
      fCtx.fill()
      fCtx.restore()
    }
  })
}

function updateRepairVisuals(){
const parts=['window','wall','door','roof'];
const icons={window:'🪟',wall:'🧱',door:'🚪',roof:'🪵'};
let progressText='Repairs: ';
  
parts.forEach(part=>{
  const item=document.getElementById('item-'+part);
  const btn=item.querySelector('button');
  
  if(repairProgress[part]===1){
    item.classList.add('completed');
    btn.textContent='Done';
    btn.disabled=true;
    progressText+=icons[part]+'✅ ';
  }else{
    item.classList.remove('completed');
    btn.textContent='Repair';
    btn.disabled=false;
    progressText+=icons[part]+'❌ ';
  }
});
  
document.getElementById('repairProgress').textContent=progressText.trim();
}

function showHealToast(){
healToast.style.display='block';
healToast.style.animation='none';
setTimeout(()=>{
  healToast.style.animation='fadeInOut 1.5s ease';
},10);
setTimeout(()=>{
  healToast.style.display='none';
},1500);
}

function showCaptureToast(){
captureToast.style.display='block';
captureToast.style.animation='none';
setTimeout(()=>{
  captureToast.style.animation='fadeInOut 2s ease';
},10);
setTimeout(()=>{
  captureToast.style.display='none';
},2000);
}

function showDamageIndicator(targetX,targetY,dmg){
  const ind=document.createElement('div')
  ind.className='damageIndicator'
  ind.textContent='-'+dmg
  ind.style.left=targetX+'px'
  ind.style.top=targetY+'px'
  world.appendChild(ind)
  setTimeout(()=>ind.remove(),800)
}

/* WOLF AI SYSTEM - Day/Night behavior */
function updateWolves(){
if(!gameStarted||gameOver) return

const wolvesNear=wolves.some(w=>Math.hypot(w.x-x,w.y-y)<300)
const showSafeZone=wolvesNear
safeZone.style.display=showSafeZone?'block':'none'

const currentSafeRadius=hasMoat?MOAT_SAFE_RADIUS:HOME_SAFE_RADIUS
const zoneSize=currentSafeRadius*2
const zoneOffset=600-currentSafeRadius
safeZone.style.width=zoneSize+'px'
safeZone.style.height=zoneSize+'px'
safeZone.style.left=zoneOffset+'px'
safeZone.style.top=zoneOffset+'px'

// Update each wolf
for(let i=wolves.length-1;i>=0;i--){
  const w=wolves[i]
  
  const denDist=Math.hypot(w.denX-w.x,w.denY-w.y)
  const playerDist=Math.hypot(x-w.x,y-w.y)
  const homeDist=Math.hypot(homeX-w.x,homeY-w.y)
  const playerHomeDist=Math.hypot(homeX-x,homeY-y)
  const playerDenDist=Math.hypot(w.denX-x,w.denY-y)
  const wolfSafeRadius=hasMoat?MOAT_SAFE_RADIUS:HOME_SAFE_RADIUS
  const playerInSafeZone=playerHomeDist<wolfSafeRadius
  const inOutpost=dens.some(d=>d.cleared&&Math.hypot(d.x-x,d.y-y)<OUTPOST_SAFE_RADIUS)
  
  let shouldBeAggressive=false
  
  // Day behavior: passive unless player too close
  if(!isNight){
    if(playerDist<WOLF_DAY_AGGRO_DIST||playerDenDist<WOLF_DAY_DEN_AGGRO_DIST){
      shouldBeAggressive=true
      w.deaggroTimer=0
    }else if(w.aggressive){
      w.deaggroTimer+=16
      if(w.deaggroTimer>=WOLF_DEAGGRO_TIME){
        w.aggressive=false
      }
    }
  }else{
    // Night behavior: aggressive in full territory
    if(denDist<WOLF_PATROL_RADIUS){
      shouldBeAggressive=true
    }
  }
  
  w.aggressive=shouldBeAggressive
  
  // Update visual state
  if(w.aggressive){
    w.el.classList.add('aggressive')
    w.el.classList.remove('passive')
    w.stateEl.textContent='👁️'
  }else{
    w.el.classList.add('passive')
    w.el.classList.remove('aggressive')
    w.stateEl.textContent='💤'
  }
  
  // Movement
  let shouldRetreat=false
  if(homeDist<wolfSafeRadius){
    shouldRetreat=true
  }else if(denDist>WOLF_PATROL_RADIUS){
    // Return to den
    const dx=w.denX-w.x
    const dy=w.denY-w.y
    const d=Math.hypot(dx,dy)
    if(d>0){
      w.x+=dx/d*WOLF_SPEED
      w.y+=dy/d*WOLF_SPEED
    }
  }else if(w.aggressive&&!playerInSafeZone&&!inOutpost&&playerDist>30){
    // Chase player
    const dx=x-w.x
    const dy=y-w.y
    if(playerDist>0){
      w.x+=dx/playerDist*WOLF_SPEED
      w.y+=dy/playerDist*WOLF_SPEED
    }
  }else{
    // Passive patrol
    w.patrolAngle+=0.02
    w.x+=Math.cos(w.patrolAngle)*w.patrolSpeed
    w.y+=Math.sin(w.patrolAngle)*w.patrolSpeed
  }
  
  if(shouldRetreat){
    const hx=homeX-w.x
    const hy=homeY-w.y
    const hd=Math.hypot(hx,hy)
    if(hd>0){
      w.x-=hx/hd*WOLF_SPEED
      w.y-=hy/hd*WOLF_SPEED
    }
  }
  
  w.el.style.left=w.x+'px'
  w.el.style.top=w.y+'px'
  
  // Attack player
  if(w.attackCooldown>0){
    w.attackCooldown--
  }else if(playerDist<50&&w.aggressive&&!gameOver&&!playerInSafeZone&&!inOutpost){
    health=Math.max(0,health-WOLF_DAMAGE)
    w.attackCooldown=60
    draw()
    
    document.getElementById('starvedOverlay').style.display='block'
    setTimeout(()=>document.getElementById('starvedOverlay').style.display='none',100)
    
    if(health<=0){
      triggerGameOver()
    }
  }
  
  w.x=Math.max(0,Math.min(WORLD_SIZE,w.x))
  w.y=Math.max(0,Math.min(WORLD_SIZE,w.y))
}
}

function checkDenCapture(){
  dens.forEach(den=>{
    if(!den.cleared){
      const denWolves=wolves.filter(w=>w.denId===den.id)
      if(denWolves.length===0){
        den.cleared=true
        den.el.textContent='🏰'
        den.el.classList.add('cleared')
        densCleared++
        
        wood+=50
        food+=20
        
        const zone=document.createElement('div')
        zone.className='outpostZone'
        zone.style.left=(den.x-80)+'px'
        zone.style.top=(den.y-80)+'px'
        world.appendChild(zone)
        
        showCaptureToast()
        updateOutpostUI()
        draw()
      }
    }
  })
}

function updateOutpostUI(){
  document.getElementById('outpostUI').textContent=`🏚️ Dens: ${densCleared}/3 Cleared`
}

function triggerGameOver(){
gameOver=true
const survived=Date.now()-gameStartTime
const mins=Math.floor(survived/60000)
const secs=Math.floor((survived%60000)/1000)

document.getElementById('finalLevel').textContent=homeLevel
document.getElementById('finalTime').textContent=`${mins}:${secs.toString().padStart(2,'0')}`
document.getElementById('finalKills').textContent=wolvesKilled
document.getElementById('finalDens').textContent=densCleared
document.getElementById('gameOverOverlay').style.display='flex'
}

/* DAY/NIGHT UPDATE */
function updateDayNight(){
if(!gameStarted) return

const elapsed=(Date.now()-cycleStartTime)%CYCLE_LENGTH
const isNightNow=elapsed>=DAY_LENGTH
const timeInPhase=isNightNow?elapsed-DAY_LENGTH:elapsed
const phaseTotal=isNightNow?NIGHT_LENGTH:DAY_LENGTH
const timeRemaining=Math.ceil((phaseTotal-timeInPhase)/1000)

if(isNightNow!==isNight){
  isNight=isNightNow
  if(isNight){
    dayNightOverlay.classList.add('night')
    visionMask.classList.add('night')
    sunMoon.textContent='🌙'
  }else{
    dayNightOverlay.classList.remove('night')
    visionMask.classList.remove('night')
    sunMoon.textContent='☀️'
  }
}

const mins=Math.floor(timeRemaining/60)
const secs=timeRemaining%60
const phaseIcon=isNight?'🌙':'☀️'
const phaseName=isNight?'Night':'Day'
document.getElementById('timeUI').innerHTML=`${phaseIcon} ${phaseName}: <span id="timeDisplay">${mins}:${secs.toString().padStart(2,'0')}</span>`

const cycleProgress=elapsed/CYCLE_LENGTH
const trackWidth=document.getElementById('sunMoonTrack').offsetWidth
sunMoon.style.left=(cycleProgress*(trackWidth-28))+'px'

if(isNight){
  const px=window.innerWidth/2
  const py=window.innerHeight/2
  visionMask.style.background=`radial-gradient(circle 200px at ${px}px ${py}px, transparent 0%, rgba(5,10,25,0.95) 100%)`
}
}

function drawMinimap(){
  mCtx.clearRect(0,0,160,160)
  
  mCtx.fillStyle='#0a150a'
  mCtx.fillRect(0,0,160,160)
  
  mCtx.strokeStyle='#1a2a1a'
  mCtx.lineWidth=1
  for(let i=0;i<160;i+=20){
    mCtx.beginPath()
    mCtx.moveTo(i,0)
    mCtx.lineTo(i,160)
    mCtx.stroke()
    mCtx.beginPath()
    mCtx.moveTo(0,i)
    mCtx.lineTo(160,i)
    mCtx.stroke()
  }
  
  const scale=160/WORLD_SIZE
  
  mCtx.fillStyle='rgba(0,0,0,0.85)'
  mCtx.fillRect(0,0,160,160)
  
  mCtx.fillStyle='#1a2a1a'
  exploredTiles.forEach(key=>{
    const [tx,ty]=key.split(',').map(Number)
    mCtx.fillRect(tx*TILE_SIZE*scale,ty*TILE_SIZE*scale,TILE_SIZE*scale,TILE_SIZE*scale)
  })
  
  dens.forEach(den=>{
    if(den.cleared){
      mCtx.fillStyle='rgba(255,204,68,0.1)'
      mCtx.beginPath()
      mCtx.arc(den.x*scale,den.y*scale,OUTPOST_SAFE_RADIUS*scale,0,Math.PI*2)
      mCtx.fill()
    }
  })
  
  mCtx.fillStyle='#4f8'
  mCtx.fillRect(homeX*scale-4,homeY*scale-4,8,8)
  
  dens.forEach(den=>{
    mCtx.fillStyle=den.cleared?'#fc4':'#f44'
    mCtx.fillRect(den.x*scale-3,den.y*scale-3,6,6)
  })
  
  mCtx.fillStyle='#fff'
  mCtx.beginPath()
  mCtx.arc(x*scale,y*scale,3,0,Math.PI*2)
  mCtx.fill()
  
  wolves.forEach(w=>{
    mCtx.fillStyle=w.aggressive?'#f44':'#f88'
    mCtx.beginPath()
    mCtx.arc(w.x*scale,w.y*scale,2,0,Math.PI*2)
    mCtx.fill()
  })
}

function draw(){
world.style.transform=`translate(${-x+window.innerWidth/2}px,${-y+window.innerHeight/2}px)`
player.style.left=x+'px'
player.style.top=y+'px'

document.getElementById('health').textContent=health
document.getElementById('wood').textContent=wood
document.getElementById('metal').textContent=metal
document.getElementById('food').textContent=food
document.getElementById('hunger').textContent=hunger
document.getElementById('level').textContent=homeLevel

const dx=homeX-x
const dy=homeY-y
const angle=Math.atan2(dy,dx)*180/Math.PI

arrow.style.transform=`rotate(${angle}deg)`
arrow.style.left='10px'
arrow.style.top='-30px'
updateCustomizationUI()
updateDayNight()
updateFog()
drawMinimap()
}

function updateHomeVisuals(){
if(homeLevel===1)home.textContent='🏚️'
if(homeLevel===2)home.textContent='🏠'
if(homeLevel===3)home.textContent='🏡'
if(homeLevel===4)home.textContent='🏘️'
if(homeLevel===5)home.textContent='🏰'

if(homeLevel>=5){
  document.getElementById('customTabBtn').style.display='block';
}else{
  document.getElementById('customTabBtn').style.display='none';
}
applyPaintFilter()
}

function applyPaintFilter(){
const filters={
  none:'none',
  red:'hue-rotate(0deg) saturate(1.5) brightness(1.1)',
  blue:'hue-rotate(220deg) saturate(1.4) brightness(1.05)',
  green:'hue-rotate(120deg) saturate(1.4) brightness(1.05)',
  yellow:'hue-rotate(60deg) saturate(1.6) brightness(1.1)',
  purple:'hue-rotate(280deg) saturate(1.5) brightness(1.05)'
};
home.style.filter=filters[houseColor] || 'none';
}

function switchTab(tab){
document.querySelectorAll('.tabBtn').forEach(b=>b.classList.remove('active'));
document.querySelectorAll('.tabContent').forEach(c=>c.classList.remove('active'));

if(tab==='repairs'){
  document.querySelector('.tabBtn:nth-child(1)').classList.add('active');
  document.getElementById('repairTab').classList.add('active');
}else{
  document.getElementById('customTabBtn').classList.add('active');
  document.getElementById('customTab').classList.add('active');
  updateCustomizationUI();
}
}

/* HUNGER SYSTEM */
function startGame(){
document.getElementById('instructionsOverlay').style.display='none';
gameStarted=true;
gameStartTime=Date.now();
cycleStartTime=Date.now();
updateFog()
spawnInitialWolves()
}

function cookFood(){
if(food<=0) return;
food-=1;
hunger=Math.min(100,hunger+30);
health=Math.min(100,health+7);
showHealToast();
if(hunger>0){
  document.getElementById('starvedOverlay').style.display='none';
}
draw();
updateRepairVisuals();
}

function depleteHunger(){
if(gameOver) return
const depletionRate=isNight?6:3;
hunger=Math.max(0,hunger-depletionRate);
if(hunger===0){
  document.getElementById('starvedOverlay').style.display='block';
}
draw();
}

setInterval(depleteHunger, 20000);

/* REPAIR SYSTEM */
function repair(part){
const cost={
window:{wood:2,metal:0},
wall:{wood:2,metal:1},
door:{wood:1,metal:2},
roof:{wood:3,metal:2}
}

const c=cost[part]
if(repairProgress[part] === 1) return
if(wood<c.wood||metal<c.metal) return

wood-=c.wood
metal-=c.metal

repairProgress[part]=1
updateRepairVisuals()

let done =
repairProgress.window +
repairProgress.wall +
repairProgress.door +
repairProgress.roof

if(done === 4){
homeLevel++
repairProgress={
window:0,
wall:0,
door:0,
roof:0
}
updateHomeVisuals()
updateRepairVisuals()
}

draw()
}

/* CUSTOMIZATION SYSTEM */
function updateCustomizationUI(){
const upgrades={
  sledgehammer:{wood:8,metal:5},
  chest:{wood:10,metal:6},
  moat:{wood:12,metal:8}
};

Object.keys(upgrades).forEach(key=>{
  const item=document.getElementById('item-'+key);
  if(!item) return;
  const btn=item.querySelector('button');
  const cost=upgrades[key];
  
  if(key==='sledgehammer' && hasSledgehammer){
    item.classList.add('completed');
    btn.textContent='Owned';
    btn.disabled=true;
  }else if(key==='chest' && hasChest){
    item.classList.add('completed');
    btn.textContent='Built';
    btn.disabled=true;
  }else if(key==='moat' && hasMoat){
    item.classList.add('completed');
    btn.textContent='Built';
    btn.disabled=true;
  }else{
    btn.disabled = wood<cost.wood || metal<cost.metal;
    if(item.classList.contains('completed')) item.classList.remove('completed');
  }
});

const cookItem=document.getElementById('item-cook');
if(cookItem){
  const cookBtn=cookItem.querySelector('button');
  cookBtn.disabled = food<=0 || (hunger>=100 && health>=100);
}

const gardenItem=document.getElementById('item-garden');
const gardenOptions=document.getElementById('gardenOptions');
const gardenBtn=document.getElementById('gardenBtn');
if(gardenColor){
  gardenItem.classList.add('completed');
  gardenOptions.style.display='none';
  gardenBtn.style.display='block';
  gardenBtn.textContent='Planted ✅';
  gardenBtn.disabled=true;
}else{
  const canCraft = wood>=4 && food>=2;
  document.querySelectorAll('#gardenOptions .colorBtn').forEach(btn=>{
    btn.style.opacity = canCraft ? '1' : '0.3';
    btn.style.pointerEvents = canCraft ? 'auto' : 'none';
  });
  gardenOptions.style.display='flex';
  gardenBtn.style.display='none';
}

document.querySelectorAll('#paintOptions .colorBtn').forEach(btn=>{
  btn.classList.toggle('selected', btn.dataset.color === houseColor);
});
}

function craftUpgrade(type){
const costs={
  sledgehammer:{wood:8,metal:5},
  chest:{wood:10,metal:6},
  moat:{wood:12,metal:8}
};

const cost=costs[type];
if(wood<cost.wood || metal<cost.metal) return;

wood-=cost.wood;
metal-=cost.metal;

if(type==='sledgehammer'){
  hasSledgehammer=true;
}else if(type==='chest'){
  hasChest=true;
  document.getElementById('chest').style.display='block';
}else if(type==='moat'){
  hasMoat=true;
  document.getElementById('moat').style.display='block';
}

updateCustomizationUI();
draw();
}

function paintHouse(color){
houseColor=color;
applyPaintFilter();
updateCustomizationUI();
}

function craftGarden(color){
if(wood<4 || food<2 || gardenColor) return;

wood-=4;
food-=2;
gardenColor=color;

const flowerMap={pink:'🌸',yellow:'🌻',red:'🌹',purple:'🌷',white:'🌼'};
const emoji=flowerMap[color];
const positions=[
  {x:homeX-60,y:homeY-40},{x:homeX+60,y:homeY-40},
  {x:homeX-80,y:homeY+20},{x:homeX+80,y:homeY+20},
  {x:homeX-40,y:homeY+90},{x:homeX+40,y:homeY+90}
];

positions.forEach((pos,i)=>{
  const flower=document.createElement('div');
  flower.className='e';
  flower.textContent=emoji;
  flower.style.left=pos.x+'px';
  flower.style.top=pos.y+'px';
  flower.style.zIndex='15';
  flower.style.transition='all 0.3s ease';
  world.appendChild(flower);
  gardenElements.push(flower);
});

updateCustomizationUI();
draw();
}

function openMenu(){
document.getElementById('homeMenu').style.display='flex'
updateRepairVisuals()
updateCustomizationUI()
}

function closeMenu(){
document.getElementById('homeMenu').style.display='none'
}

function checkHome(){
if(Math.hypot(homeX-x,homeY-y)<120) openMenu()
}

/* COMBAT SYSTEM */
function attack(){
if(gameOver) return

let hitSomething=false

for(let i=wolves.length-1;i>=0;i--){
  const w=wolves[i]
  if(Math.hypot(w.x-x,w.y-y)<80){
    w.hp-=WOLF_ATTACK_DAMAGE
    w.aggressive=true
    w.deaggroTimer=0
    showDamageIndicator(w.x,w.y,WOLF_ATTACK_DAMAGE)
    hitSomething=true
    
    if(w.hp<=0){
      w.el.remove()
      const den=dens.find(d=>d.id===w.denId)
      if(den){
        const idx=den.wolves.indexOf(w)
        if(idx>=0) den.wolves.splice(idx,1)
      }
      wolves.splice(i,1)
      wolvesKilled++
      
      if(Math.random()<0.3){
        const fx=x+(Math.random()-0.5)*100
        const fy=y+(Math.random()-0.5)*100
        spawn('food','🍖',fx,fy)
      }
      checkDenCapture()
    }
    break
  }
}

if(!hitSomething) checkHome()
draw()
}

/* ACTION - Gather */
document.getElementById('act').onclick=()=>{
if(gameOver) return

let target=null

for(const e of entities){
if(Math.hypot(e.x-x,e.y-y)<80){
target=e
break
}
}

if(!target){
checkHome()
draw()
return
}

target.hp--

if(target.hp<=0){
const bonus = hasSledgehammer ? 1 : 0;
if(target.type==='tree'){
  wood+=3+bonus
}
if(target.type==='rock'){
  metal+=2+bonus
}
if(target.type==='food')food+=1

target.el.remove()
entities=entities.filter(v=>v!==target)
}

draw()
}

document.getElementById('attackBtn').onclick=attack

/* JOYSTICK */
let mx=0,my=0,drag=false
const js=document.getElementById('joystick')
const st=document.getElementById('stick')
const SPEED_MULTIPLIER = 2
const DEADZONE = 0.25
const MAX_DRAG = 50

js.addEventListener('touchstart',()=>drag=true)

window.addEventListener('touchend',()=>{
drag=false;mx=my=0
st.style.left='45px'
st.style.top='45px'
})

window.addEventListener('touchmove',e=>{
if(!drag)return

const t=e.touches[0]
const r=js.getBoundingClientRect()

let dx=t.clientX-r.left-70
let dy=t.clientY-r.top-70

const d=Math.hypot(dx,dy)

if(d>MAX_DRAG){dx=dx/d*MAX_DRAG;dy=dy/d*MAX_DRAG}

st.style.left=(dx+45)+'px'
st.style.top=(dy+45)+'px'

mx=dx/MAX_DRAG
my=dy/MAX_DRAG

if(Math.hypot(mx,my) < DEADZONE){
  mx=0
  my=0
}
})

// Keyboard controls
window.addEventListener('keydown',e=>{
  if(e.key===' ') attack()
  if(e.key==='e'||e.key==='E') document.getElementById('act').click()
})

function loop(){
if(!gameOver){
  x+=mx*SPEED_MULTIPLIER
  y+=my*SPEED_MULTIPLIER
  x=Math.max(0,Math.min(WORLD_SIZE,x))
  y=Math.max(0,Math.min(WORLD_SIZE,y))
  updateWolves()
}
draw()
requestAnimationFrame(loop)
}

updateRepairVisuals()
updateHomeVisuals()
updateOutpostUI()
draw()
loop()

</script>
</body>
</html>