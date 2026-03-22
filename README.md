<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#0a1a06">
<title>Jungle Racer Pro</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Rajdhani:wght@400;600;700&display=swap');
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
html,body{width:100%;height:100%;overflow:hidden;background:#050f03;touch-action:none;font-family:'Rajdhani',sans-serif;}
body{display:flex;align-items:center;justify-content:center;}
#wrap{position:relative;width:420px;height:720px;overflow:hidden;flex-shrink:0;}
canvas{display:block;}
/* HUD */
#hud{position:absolute;top:0;left:0;right:0;z-index:20;pointer-events:none;padding:12px 14px 0;}
#hud-top{display:flex;justify-content:space-between;align-items:flex-start;gap:6px;}
.hb{background:rgba(0,0,0,.72);border:1px solid rgba(255,255,255,.13);border-radius:11px;padding:5px 12px;backdrop-filter:blur(12px);}
#lbl-sc{font-size:8px;letter-spacing:3px;color:rgba(255,255,255,.38);text-transform:uppercase;}
#val-sc{font-family:'Bebas Neue',sans-serif;font-size:34px;color:#fff;line-height:1;letter-spacing:2px;}
#coin-hud{display:flex;align-items:center;gap:5px;padding:6px 11px;}
#coin-icon{font-size:18px;}
#val-coins{font-family:'Bebas Neue',sans-serif;font-size:26px;color:#ffd700;line-height:1;}
#val-km{font-family:'Bebas Neue',sans-serif;font-size:22px;color:#44ffaa;line-height:1;}
#lbl-km{font-size:8px;letter-spacing:2px;color:rgba(255,255,255,.36);text-transform:uppercase;}
#hrt-row{display:flex;gap:4px;align-items:center;margin-top:5px;justify-content:center;}
.hrt{font-size:16px;transition:opacity .3s,filter .3s;}
.hrt.dead{opacity:.15;filter:grayscale(1);}
/* Zone + weather */
#zone-row{display:flex;justify-content:center;align-items:center;gap:8px;margin-top:5px;}
#zone-pill{background:rgba(0,0,0,.6);border:1px solid rgba(255,255,255,.15);border-radius:16px;padding:3px 12px;display:flex;align-items:center;gap:5px;}
#zfl{font-size:14px;}
#znm{font-family:'Bebas Neue',sans-serif;font-size:12px;color:rgba(255,255,255,.72);letter-spacing:2px;}
#weather-icon{font-size:18px;}
/* Power-ups bar */
#pwr-bar{position:absolute;top:130px;right:14px;z-index:20;pointer-events:none;display:flex;flex-direction:column;gap:6px;}
.pwr-slot{width:46px;height:46px;border-radius:12px;border:2px solid rgba(255,255,255,.15);background:rgba(0,0,0,.6);display:flex;align-items:center;justify-content:center;font-size:22px;position:relative;backdrop-filter:blur(8px);}
.pwr-slot.active{border-color:#44ff88;box-shadow:0 0 12px #44ff8844;}
.pwr-timer{position:absolute;bottom:-2px;left:0;right:0;height:3px;background:#44ff88;border-radius:2px;transform-origin:left;}
/* Speed */
#spd-wrap{position:absolute;bottom:88px;left:14px;z-index:20;pointer-events:none;}
#spd-ring-label{font-size:8px;letter-spacing:2px;color:rgba(255,255,255,.35);text-transform:uppercase;text-align:center;margin-bottom:2px;}
#spd-num{font-family:'Bebas Neue',sans-serif;font-size:28px;color:#fff;text-align:center;line-height:1;}
#spd-unit{font-size:9px;color:rgba(255,255,255,.4);text-align:center;letter-spacing:1px;}
/* Leaderboard */
#lb-wrap{position:absolute;bottom:88px;right:14px;z-index:20;pointer-events:none;background:rgba(0,0,0,.62);border:1px solid rgba(255,255,255,.12);border-radius:10px;padding:6px 10px;min-width:100px;}
#lb-title{font-size:8px;letter-spacing:2px;color:rgba(255,255,255,.4);text-transform:uppercase;margin-bottom:4px;}
.lb-row{display:flex;justify-content:space-between;gap:8px;font-size:11px;font-weight:600;padding:1px 0;}
.lb-rank{color:rgba(255,255,255,.4);}
.lb-score{color:#ffd700;}
.lb-row.you .lb-score{color:#44ff88;}
/* Touch */
.tbtn{position:absolute;bottom:82px;width:74px;height:74px;border-radius:50%;border:2px solid rgba(255,255,255,.18);background:rgba(0,0,0,.4);display:flex;align-items:center;justify-content:center;font-size:24px;color:rgba(255,255,255,.55);z-index:30;cursor:pointer;transition:background .12s,transform .1s;}
.tbtn:active{background:rgba(255,255,255,.14);transform:scale(.92);}
#tbl{left:100px;}#tbr{right:100px;}
/* Boost button */
#boost-btn{position:absolute;bottom:82px;left:50%;transform:translateX(-50%);width:64px;height:64px;border-radius:50%;border:2px solid #ffcc00;background:rgba(255,200,0,.15);display:flex;align-items:center;justify-content:center;font-size:28px;z-index:30;cursor:pointer;transition:all .12s;}
#boost-btn:active{background:rgba(255,200,0,.35);transform:translateX(-50%) scale(.9);}
#boost-cd{position:absolute;bottom:-16px;font-size:9px;color:#ffcc00;letter-spacing:1px;white-space:nowrap;}
/* Overlays */
#overlay-main{position:absolute;inset:0;z-index:60;background:rgba(0,6,2,.97);display:flex;flex-direction:column;align-items:center;justify-content:center;overflow-y:auto;}
#overlay-main.hidden{display:none;}
/* Main menu */
#screen-menu{display:flex;flex-direction:column;align-items:center;gap:14px;padding:30px 20px;width:100%;}
#screen-menu h1{font-family:'Bebas Neue',sans-serif;font-size:68px;color:#44ff88;letter-spacing:7px;line-height:1;text-align:center;text-shadow:0 0 40px #44ff8866;}
.menu-sub{font-size:12px;color:rgba(255,255,255,.35);letter-spacing:4px;text-transform:uppercase;}
.menu-btn{width:100%;max-width:300px;padding:14px 0;border:2px solid #44ff88;background:transparent;color:#44ff88;font-family:'Bebas Neue',sans-serif;font-size:22px;letter-spacing:5px;border-radius:8px;cursor:pointer;transition:all .18s;}
.menu-btn:hover,.menu-btn:active{background:#44ff88;color:#001a08;box-shadow:0 0 28px #44ff8888;}
.menu-btn.secondary{border-color:rgba(255,255,255,.3);color:rgba(255,255,255,.6);font-size:18px;}
.menu-btn.secondary:hover{background:rgba(255,255,255,.1);color:#fff;box-shadow:none;}
/* Death screen */
#screen-dead{display:none;flex-direction:column;align-items:center;gap:14px;padding:30px 20px;width:100%;}
#screen-dead h2{font-family:'Bebas Neue',sans-serif;font-size:58px;color:#ff4444;letter-spacing:5px;text-shadow:0 0 30px #ff444488;}
.dead-score{font-family:'Bebas Neue',sans-serif;font-size:50px;color:#ffd700;letter-spacing:4px;}
.dead-best{font-size:13px;color:#44ff88;letter-spacing:3px;}
/* Car selector */
#screen-cars{display:none;flex-direction:column;align-items:center;gap:12px;padding:20px;width:100%;}
#screen-cars h2{font-family:'Bebas Neue',sans-serif;font-size:44px;color:#fff;letter-spacing:4px;}
.cars-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;width:100%;}
.car-card{border:2px solid rgba(255,255,255,.15);border-radius:14px;padding:12px 8px;display:flex;flex-direction:column;align-items:center;gap:6px;cursor:pointer;transition:all .18s;background:rgba(255,255,255,.04);}
.car-card.selected{border-color:#44ff88;background:rgba(68,255,136,.08);box-shadow:0 0 16px #44ff8833;}
.car-card.locked{opacity:.55;filter:grayscale(.6);}
.car-preview{width:50px;height:90px;display:flex;align-items:center;justify-content:center;}
.car-name{font-family:'Bebas Neue',sans-serif;font-size:13px;color:#fff;letter-spacing:1px;}
.car-price{font-size:11px;color:#ffd700;font-weight:600;}
.car-price.owned{color:#44ff88;}
.car-stats{display:flex;gap:4px;width:100%;}
.stat-bar-wrap{flex:1;}
.stat-bar{height:3px;background:rgba(255,255,255,.12);border-radius:2px;margin-top:2px;}
.stat-fill{height:100%;border-radius:2px;background:#44ff88;}
.stat-label{font-size:8px;color:rgba(255,255,255,.4);letter-spacing:1px;}
/* Shop */
#screen-shop{display:none;flex-direction:column;align-items:center;gap:10px;padding:20px;width:100%;}
#screen-shop h2{font-family:'Bebas Neue',sans-serif;font-size:44px;color:#ffd700;letter-spacing:4px;}
.shop-coins{font-size:18px;color:#ffd700;font-weight:700;margin-bottom:4px;}
.shop-item{width:100%;border:1px solid rgba(255,255,255,.12);border-radius:12px;padding:12px 14px;display:flex;align-items:center;gap:12px;background:rgba(255,255,255,.04);cursor:pointer;transition:background .15s;}
.shop-item:hover{background:rgba(255,255,255,.08);}
.shop-item-icon{font-size:28px;width:44px;text-align:center;}
.shop-item-info{flex:1;}
.shop-item-name{font-size:15px;font-weight:700;color:#fff;}
.shop-item-desc{font-size:11px;color:rgba(255,255,255,.45);}
.shop-item-price{font-size:14px;color:#ffd700;font-weight:700;}
/* Leaderboard screen */
#screen-lb{display:none;flex-direction:column;align-items:center;gap:10px;padding:20px;width:100%;}
#screen-lb h2{font-family:'Bebas Neue',sans-serif;font-size:44px;color:#ffd700;letter-spacing:4px;}
.lb-item{width:100%;border:1px solid rgba(255,255,255,.1);border-radius:10px;padding:10px 14px;display:flex;align-items:center;gap:12px;background:rgba(255,255,255,.04);}
.lb-item.you{border-color:#44ff88;background:rgba(68,255,136,.06);}
.lb-pos{font-family:'Bebas Neue',sans-serif;font-size:22px;color:rgba(255,255,255,.4);width:28px;}
.lb-pos.gold{color:#ffd700;}
.lb-pos.silver{color:#aaa;}
.lb-pos.bronze{color:#cd7f32;}
.lb-entry-name{flex:1;font-size:15px;font-weight:700;color:#fff;}
.lb-entry-score{font-family:'Bebas Neue',sans-serif;font-size:22px;color:#ffd700;}
/* Hit flash */
#hit-flash{position:absolute;inset:0;z-index:45;pointer-events:none;background:rgba(255,50,0,0);transition:background .05s;}
#hit-flash.show{background:rgba(255,50,0,.3);}
/* Boss alert */
#boss-alert{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);z-index:40;font-family:'Bebas Neue',sans-serif;font-size:48px;color:#ff4444;letter-spacing:5px;text-shadow:0 0 30px #ff4444;pointer-events:none;opacity:0;transition:opacity .3s;}
/* Power-up pickup toast */
#pwr-toast{position:absolute;top:160px;left:50%;transform:translateX(-50%);z-index:40;background:rgba(0,0,0,.8);border:1px solid #44ff88;border-radius:20px;padding:6px 18px;font-size:14px;color:#44ff88;font-weight:700;letter-spacing:2px;pointer-events:none;opacity:0;transition:opacity .3s;}
/* Weather overlay */
#rain-canvas{position:absolute;inset:0;z-index:22;pointer-events:none;opacity:0;transition:opacity 1s;}
#lbar{position:absolute;top:0;left:0;height:3px;background:#44ff88;width:0;z-index:70;border-radius:0 2px 2px 0;}
</style>
</head>
<body>
<div id="wrap">
  <canvas id="c" width="420" height="720"></canvas>
  <canvas id="rain-canvas" width="420" height="720"></canvas>

  <!-- HUD -->
  <div id="hud">
    <div id="hud-top">
      <div class="hb"><div id="lbl-sc">Score</div><div id="val-sc">0</div></div>
      <div class="hb" id="coin-hud"><span id="coin-icon">🪙</span><div id="val-coins">0</div></div>
      <div class="hb"><div id="val-km">0m</div><div id="lbl-km">dist</div></div>
    </div>
    <div id="hrt-row"><span class="hrt" id="h1">❤️</span><span class="hrt" id="h2">❤️</span><span class="hrt" id="h3">❤️</span></div>
    <div id="zone-row"><div id="zone-pill"><span id="zfl">🌴</span><span id="znm">JUNGLE ZONE</span></div><span id="weather-icon"></span></div>
  </div>

  <!-- Power-ups -->
  <div id="pwr-bar">
    <div class="pwr-slot" id="pwr-shield" title="Shield">🛡️<div class="pwr-timer" id="shield-timer" style="width:0%"></div></div>
    <div class="pwr-slot" id="pwr-magnet" title="Magnet">🧲<div class="pwr-timer" id="magnet-timer" style="width:0%"></div></div>
  </div>

  <!-- Speed -->
  <div id="spd-wrap">
    <div id="spd-ring-label">Speed</div>
    <div id="spd-num">0</div>
    <div id="spd-unit">km/h</div>
  </div>

  <!-- Leaderboard mini -->
  <div id="lb-wrap">
    <div id="lb-title">🏆 Best</div>
    <div id="lb-mini"></div>
  </div>

  <!-- Touch controls -->
  <div class="tbtn" id="tbl">◀</div>
  <div class="tbtn" id="tbr">▶</div>
  <div id="boost-btn">⚡<div id="boost-cd"></div></div>

  <!-- Effects -->
  <div id="hit-flash"></div>
  <div id="boss-alert">⚠ BOSS!</div>
  <div id="pwr-toast"></div>
  <div id="lbar"></div>

  <!-- SCREENS -->
  <div id="overlay-main">
    <!-- Main Menu -->
    <div id="screen-menu">
      <h1>JUNGLE<br>RACER<br>PRO</h1>
      <div class="menu-sub">Ultimate Edition</div>
      <button class="menu-btn" id="btn-play">▶  START RACE</button>
      <button class="menu-btn secondary" id="btn-cars">🚗  SELECT CAR</button>
      <button class="menu-btn secondary" id="btn-shop">🛒  SHOP</button>
      <button class="menu-btn secondary" id="btn-lb">🏆  LEADERBOARD</button>
      <div class="menu-sub" style="font-size:10px;">ARROWS · WASD · SWIPE · BUTTONS</div>
    </div>

    <!-- Death Screen -->
    <div id="screen-dead">
      <h2>GAME OVER</h2>
      <div id="dead-score" class="dead-score">0</div>
      <div id="dead-best" class="dead-best"></div>
      <div id="dead-coins" style="font-size:14px;color:#ffd700;margin-top:4px;"></div>
      <button class="menu-btn" id="btn-retry" style="margin-top:10px;">▶  RACE AGAIN</button>
      <button class="menu-btn secondary" id="btn-menu">MAIN MENU</button>
    </div>

    <!-- Car Selector -->
    <div id="screen-cars">
      <h2>SELECT CAR</h2>
      <div id="total-coins-cars" class="shop-coins"></div>
      <div class="cars-grid" id="cars-grid"></div>
      <button class="menu-btn" id="btn-cars-play" style="margin-top:8px;">▶  RACE THIS CAR</button>
      <button class="menu-btn secondary" id="btn-cars-back">← BACK</button>
    </div>

    <!-- Shop -->
    <div id="screen-shop">
      <h2>SHOP</h2>
      <div id="shop-coins-disp" class="shop-coins"></div>
      <div id="shop-items"></div>
      <button class="menu-btn secondary" id="btn-shop-back" style="margin-top:8px;">← BACK</button>
    </div>

    <!-- Leaderboard -->
    <div id="screen-lb">
      <h2>LEADERBOARD</h2>
      <div id="lb-entries"></div>
      <button class="menu-btn secondary" id="btn-lb-back" style="margin-top:8px;">← BACK</button>
    </div>
  </div>
</div>

<script>
'use strict';
/* ═══════════════════════════════════════════════════════════════
   JUNGLE RACER PRO — Massive Update
   Features: 6 cars, coins, power-ups, shop, boss, leaderboard,
             weather (rain/night), explosions, 5 zones, crowds
   Architecture: render() = pure read, ALL mutations in tick()
   ═══════════════════════════════════════════════════════════════ */

const CV=document.getElementById('c');
const G=CV.getContext('2d',{alpha:false,willReadFrequently:false});
const RC=document.getElementById('rain-canvas');
const RG=RC.getContext('2d',{alpha:true});
const W=420,H=720;

// ── Road ──
const RL=78,RR=342,RW=RR-RL,LW=RW/3;
const LANES=[RL+LW*.5,RL+LW*1.5,RL+LW*2.5];
const GY=H-100;

// ── State ──
let gs='menu';
let score=0,best=0,lives=3,speed=3.5,MAXSPD=10.5;
let roadOff=0,tickN=0,zoneIdx=0,spawnClock=0;
let prevTs=0,accumT=0;
const FDT=1000/60;
let frameID=null,titleID=null;

// ── Persistent data (localStorage) ──
let totalCoins=0,ownedCars=[0],selectedCar=0,highScores=[];
function loadData(){
  try{
    totalCoins=parseInt(localStorage.getItem('jrp_coins')||'0');
    ownedCars=JSON.parse(localStorage.getItem('jrp_owned')||'[0]');
    selectedCar=parseInt(localStorage.getItem('jrp_car')||'0');
    highScores=JSON.parse(localStorage.getItem('jrp_scores')||'[]');
  }catch(e){}
}
function saveData(){
  try{
    localStorage.setItem('jrp_coins',totalCoins);
    localStorage.setItem('jrp_owned',JSON.stringify(ownedCars));
    localStorage.setItem('jrp_car',selectedCar);
    localStorage.setItem('jrp_scores',JSON.stringify(highScores));
  }catch(e){}
}
loadData();

// ── CAR DEFINITIONS ──
const CAR_DEFS=[
  {name:'RACER',  price:0,    color:'#cc1a06',dark:'#881204',acc:'#e84022',speed:1.0,handle:1.0,icon:'🚗'},
  {name:'STEALTH',price:800,  color:'#1a1a2e',dark:'#0d0d1e',acc:'#4455cc',speed:1.1,handle:1.2,icon:'🏎️'},
  {name:'BLAZE',  price:1200, color:'#ff6600',dark:'#cc4400',acc:'#ffaa00',speed:1.3,handle:0.9,icon:'🚙'},
  {name:'VIPER',  price:1800, color:'#006600',dark:'#004400',acc:'#00cc44',speed:1.2,handle:1.3,icon:'🏎'},
  {name:'CHROME', price:2500, color:'#888',   dark:'#444',   acc:'#fff',   speed:1.0,handle:1.4,icon:'🚘'},
  {name:'INFERNO',price:4000, color:'#880000',dark:'#440000',acc:'#ff2200',speed:1.5,handle:0.8,icon:'🔥'},
];

// ── ZONES ──
const ZONES=[
  {flag:'🌴',name:'JUNGLE',   sky:'#0d1f08',side:'#0a1806',road:'#1e1e1e',weather:'clear'},
  {flag:'🏜️',name:'DESERT',  sky:'#1a0e06',side:'#160a04',road:'#2a2010',weather:'clear'},
  {flag:'🌧️',name:'STORM',   sky:'#0a0e18',side:'#080c14',road:'#181820',weather:'rain'},
  {flag:'🌆',name:'CITY NIGHT',sky:'#04040e',side:'#020212',road:'#14141e',weather:'night'},
  {flag:'🌋',name:'VOLCANO',  sky:'#180400',side:'#100200',road:'#201008',weather:'clear'},
];
let zone=ZONES[0];

// ── Power-up state ──
let shieldActive=0,magnetActive=0,boostCooldown=0,boostActive=0;
let sessionCoins=0;

// ── Boss ──
let boss=null,bossAlertTimer=0;

// ── Weather ──
let rainDrops=[],isNight=false,lightningTimer=0;

// ── Random pool ──
const RP=new Float32Array(512);
let ri=0;
function rfill(){for(let i=0;i<512;i++)RP[i]=Math.random();ri=0;}
function rnd(){if(ri>=511)rfill();return RP[ri++];}
function rndR(a,b){return a+rnd()*(b-a);}
rfill();

// ── Car object ──
let car={};
function initCar(){
  const def=CAR_DEFS[selectedCar];
  car={x:LANES[1],targetX:LANES[1],lane:1,y:GY,w:44,h:88,
       wspin:0,inv:0,tilt:0,
       col:def.color,dark:def.dark,acc:def.acc};
}

// ── Arrays ──
let obs=[],parts=[],treesL=[],treesR=[],coinObjs=[],crowdL=[],crowdR=[];
let exhaustQ=[],sLines=[];

// ── Trees ──
const TV=['palm','broad','fern','tall','banana','mangrove'];
function mkTree(side,y){
  return{x:side==='L'?9+rnd()*60:RR+12+rnd()*56,y,side,
    v:TV[Math.floor(rnd()*TV.length)],s:.6+rnd()*.72,
    sw:rnd()*Math.PI*2,ss:.006+rnd()*.012,hue:(rnd()*28-14)|0};
}

// ── Crowd figures ──
function mkCrowd(side,y){
  return{x:side==='L'?8+rnd()*40:RR+18+rnd()*40,y,side,
    color:`hsl(${rnd()*360|0},60%,${45+rnd()*30|0}%)`,
    skinTone:`hsl(${20+rnd()*30|0},${40+rnd()*30|0}%,${50+rnd()*20|0}%)`,
    armAngle:rnd()*Math.PI,waveSpeed:.04+rnd()*.06,shirt:rnd()>.5};
}

/* ═══════════ INPUT ═══════════ */
const K={};
function shiftLane(d){
  const def=CAR_DEFS[selectedCar];
  const nl=Math.max(0,Math.min(2,car.lane+d));
  if(nl===car.lane)return;
  car.lane=nl;car.targetX=LANES[nl];
  car.x+=(car.targetX-car.x)*(.28*def.handle);
  car.tilt=d*.08;
  for(let i=0;i<8;i++){
    const a=Math.PI+(rnd()-.5)*1.2;
    parts.push({x:car.x,y:car.y+car.h*.42,vx:Math.cos(a)*(2+rnd()*3)*d,vy:Math.sin(a)*(0.5+rnd()),life:.75,col:'#aaa',r:1.8+rnd()*2.4,shadow:false,isExh:false});
  }
}
function doBoost(){
  if(boostCooldown>0)return;
  boostActive=120;boostCooldown=420;
  showPwrToast('⚡ BOOST!');
}
document.addEventListener('keydown',e=>{
  if(K[e.code])return;K[e.code]=true;
  if(gs!=='running')return;
  if(e.code==='ArrowLeft'||e.code==='KeyA')shiftLane(-1);
  if(e.code==='ArrowRight'||e.code==='KeyD')shiftLane(1);
  if(e.code==='ArrowUp'||e.code==='KeyW'||e.code==='Space'){e.preventDefault();doBoost();}
});
document.addEventListener('keyup',e=>{K[e.code]=false;});

let swX=0,swY=0,swFired=false;
CV.addEventListener('touchstart',e=>{e.preventDefault();swX=e.touches[0].clientX;swY=e.touches[0].clientY;swFired=false;},{passive:false});
CV.addEventListener('touchmove',e=>{
  e.preventDefault();if(gs!=='running'||swFired)return;
  const dx=e.touches[0].clientX-swX,dy=e.touches[0].clientY-swY;
  if(Math.abs(dx)>20&&Math.abs(dx)>Math.abs(dy)){swFired=true;shiftLane(dx>0?1:-1);}
},{passive:false});

function mkTBtn(id,dir){
  const el=document.getElementById(id);
  let held=false,timer=null;
  const go=()=>{if(gs!=='running')return;shiftLane(dir);held=true;clearInterval(timer);timer=setInterval(()=>{if(held&&gs==='running')shiftLane(dir);},250);};
  const stop=()=>{held=false;clearInterval(timer);};
  el.addEventListener('touchstart',e=>{e.preventDefault();go();},{passive:false});
  el.addEventListener('touchend',e=>{e.preventDefault();stop();},{passive:false});
  el.addEventListener('touchcancel',e=>{e.preventDefault();stop();},{passive:false});
  el.addEventListener('mousedown',go);el.addEventListener('mouseup',stop);el.addEventListener('mouseleave',stop);
}
mkTBtn('tbl',-1);mkTBtn('tbr',1);
document.getElementById('boost-btn').addEventListener('click',doBoost);
document.getElementById('boost-btn').addEventListener('touchstart',e=>{e.preventDefault();doBoost();},{passive:false});

/* ═══════════ HUD ═══════════ */
function updHUD(){
  document.getElementById('val-sc').textContent=Math.floor(score);
  document.getElementById('val-km').textContent=Math.floor(score*.85)+'m';
  document.getElementById('val-coins').textContent=sessionCoins;
  for(let i=1;i<=3;i++)document.getElementById('h'+i).classList.toggle('dead',i>lives);
  const pct=Math.min((speed-3.5)/(MAXSPD-3.5),1)*100;
  document.getElementById('spd-num').textContent=Math.floor(speed*30);
  // Boost button cooldown
  const bcd=document.getElementById('boost-cd');
  if(boostCooldown>0)bcd.textContent=Math.ceil(boostCooldown/60)+'s';
  else bcd.textContent='';
  // Shield/magnet timers
  document.getElementById('shield-timer').style.width=shieldActive>0?(shieldActive/300*100)+'%':'0%';
  document.getElementById('magnet-timer').style.width=magnetActive>0?(magnetActive/480*100)+'%':'0%';
  document.getElementById('pwr-shield').classList.toggle('active',shieldActive>0);
  document.getElementById('pwr-magnet').classList.toggle('active',magnetActive>0);
  // Mini leaderboard
  updMiniLB();
}
function updMiniLB(){
  const top=highScores.slice(0,3);
  const el=document.getElementById('lb-mini');
  el.innerHTML=top.map((s,i)=>`<div class="lb-row${Math.floor(s)===Math.floor(score)&&gs==='running'?' you':''}"><span class="lb-rank">#${i+1}</span><span class="lb-score">${Math.floor(s)}</span></div>`).join('');
}
function setZone(z){
  document.getElementById('zfl').textContent=z.flag;
  document.getElementById('znm').textContent=z.name+' ZONE';
  isNight=z.weather==='night';
  const wIcon=z.weather==='rain'?'🌧️':z.weather==='night'?'🌙':'';
  document.getElementById('weather-icon').textContent=wIcon;
  // Rain canvas
  RC.style.opacity=z.weather==='rain'?'.7':'0';
  if(z.weather==='rain'&&rainDrops.length===0)initRain();
}
function showPwrToast(msg){
  const t=document.getElementById('pwr-toast');
  t.textContent=msg;t.style.opacity='1';
  clearTimeout(t._to);t._to=setTimeout(()=>t.style.opacity='0',1600);
}
function flashHit(){
  const el=document.getElementById('hit-flash');
  el.classList.add('show');setTimeout(()=>el.classList.remove('show'),120);
}

/* ═══════════ RAIN ═══════════ */
function initRain(){
  rainDrops=[];
  for(let i=0;i<120;i++){
    rainDrops.push({x:rndR(0,W),y:rndR(0,H),len:rndR(12,28),speed:rndR(8,18),alpha:rndR(.3,.7)});
  }
}
function drawRain(){
  RG.clearRect(0,0,W,H);
  if(zone.weather!=='rain')return;
  RG.strokeStyle='rgba(150,190,255,.6)';
  RG.lineWidth=1;
  for(let i=0;i<rainDrops.length;i++){
    const d=rainDrops[i];
    RG.globalAlpha=d.alpha;
    RG.beginPath();RG.moveTo(d.x,d.y);RG.lineTo(d.x+2,d.y+d.len);RG.stroke();
    d.y+=d.speed;d.x+=1.5;
    if(d.y>H){d.y=-d.len;d.x=rndR(0,W);}
  }
  RG.globalAlpha=1;
  // Lightning flash
  if(lightningTimer===1){
    RG.fillStyle='rgba(200,220,255,.22)';RG.fillRect(0,0,W,H);
  }
}

/* ═══════════ SKYGRAD CACHE ═══════════ */
let skyGrad=null,skyZRef=null,skyNightGrad=null;
function getSky(){
  if(zone===skyZRef&&skyGrad)return skyGrad;
  skyZRef=zone;
  if(isNight){
    skyGrad=G.createLinearGradient(0,0,0,H*.7);
    skyGrad.addColorStop(0,'#000010');skyGrad.addColorStop(1,zone.sky);
  }else{
    skyGrad=G.createLinearGradient(0,0,0,H*.7);
    skyGrad.addColorStop(0,'#000000');skyGrad.addColorStop(1,zone.sky);
  }
  return skyGrad;
}

/* ═══════════ ROAD ═══════════ */
function drawRoad(){
  G.fillStyle=getSky();G.fillRect(0,0,W,H);

  // Night stars
  if(isNight){
    G.fillStyle='rgba(255,255,255,.8)';
    for(let s=0;s<60;s++){
      const sx=(s*73+tickN*.02)%W,sy=(s*41)%200;
      G.globalAlpha=.5+.5*Math.sin(tickN*.04+s);
      G.fillRect(sx,sy,1.2,1.2);
    }
    G.globalAlpha=1;
  }

  G.fillStyle=zone.side;G.fillRect(0,0,RL,H);G.fillRect(RR,0,W-RR,H);

  // Side depth
  let sg=G.createLinearGradient(RL,0,RL+20,0);
  sg.addColorStop(0,'rgba(0,0,0,.6)');sg.addColorStop(1,'transparent');
  G.fillStyle=sg;G.fillRect(RL,0,20,H);
  sg=G.createLinearGradient(RR-20,0,RR,0);
  sg.addColorStop(0,'transparent');sg.addColorStop(1,'rgba(0,0,0,.6)');
  G.fillStyle=sg;G.fillRect(RR-20,0,20,H);

  G.fillStyle=zone.road;G.fillRect(RL,0,RW,H);

  // Asphalt grain
  const gOff=(roadOff*.38|0)%16;
  G.fillStyle='rgba(0,0,0,.042)';
  for(let y=-gOff;y<H;y+=16)G.fillRect(RL,y,RW,7);

  // Road puddles in rain
  if(zone.weather==='rain'){
    G.fillStyle='rgba(100,140,200,.12)';
    for(let p=0;p<4;p++){
      const px2=RL+15+p*(RW/4),py=((tickN*speed*.5+p*180)|0)%H;
      G.beginPath();G.ellipse(px2,py,20,8,0,0,Math.PI*2);G.fill();
    }
  }

  // City neon reflections on road
  if(isNight){
    const neonColors=['rgba(255,0,120,.06)','rgba(0,200,255,.06)','rgba(180,0,255,.06)'];
    neonColors.forEach((c,i)=>{
      G.fillStyle=c;G.fillRect(RL+i*(RW/3),0,RW/3,H);
    });
  }

  G.strokeStyle='rgba(255,255,255,.82)';G.lineWidth=3;G.setLineDash([]);
  G.beginPath();G.moveTo(RL+4,0);G.lineTo(RL+4,H);G.stroke();
  G.beginPath();G.moveTo(RR-4,0);G.lineTo(RR-4,H);G.stroke();
  G.strokeStyle='rgba(255,200,0,.5)';G.lineWidth=2;
  G.beginPath();G.moveTo(RL+16,0);G.lineTo(RL+16,H);G.stroke();
  G.beginPath();G.moveTo(RR-16,0);G.lineTo(RR-16,H);G.stroke();

  const dL=36,dG=24,dOff=roadOff%(dL+dG);
  G.strokeStyle='rgba(255,255,255,.5)';G.lineWidth=2.2;
  G.setLineDash([dL,dG]);G.lineDashOffset=-dOff;
  G.beginPath();G.moveTo(RL+LW,0);G.lineTo(RL+LW,H);G.stroke();
  G.beginPath();G.moveTo(RL+LW*2,0);G.lineTo(RL+LW*2,H);G.stroke();
  G.setLineDash([]);
}

/* ═══════════ TREES ═══════════ */
function drawTree(t){
  G.save();G.translate(t.x,t.y);G.lineCap='round';
  const s=t.s,sw=Math.sin(t.sw)*2.8*s,h=t.hue;
  switch(t.v){
    case 'palm':_palm(s,sw,h);break;case 'broad':_broad(s,sw,h);break;
    case 'fern':_fern(s,sw,h);break;case 'tall':_tall(s,sw,h);break;
    case 'banana':_banana(s,sw,h);break;case 'mangrove':_mang(s,sw,h);break;
  }
  G.restore();
}
function _palm(s,sw,h){const ht=90*s;G.strokeStyle=`hsl(${30+h},50%,23%)`;G.lineWidth=6*s;G.beginPath();G.moveTo(0,0);G.bezierCurveTo(sw*1.4,-ht*.32,sw*2.9+5*s,-ht*.64,sw*3.9+9*s,-ht);G.stroke();const px=sw*3.9+9*s,py=-ht;for(let i=0;i<8;i++){const a=(i/8)*Math.PI*2+sw*.04,len=(42+Math.sin(i)*8)*s,ex=px+Math.cos(a)*len,ey=py+Math.sin(a)*len*.5,mx=px+Math.cos(a)*len*.42,my=py+Math.sin(a+.28)*len*.42-11*s;G.fillStyle=`hsl(${108+h+i*2},65%,${27+i*2}%)`;G.beginPath();G.moveTo(px,py);G.quadraticCurveTo(mx,my,ex,ey);G.quadraticCurveTo(mx+4*s*Math.sin(a),my+3*s,px,py);G.fill();}G.fillStyle=`hsl(${35+h},60%,25%)`;for(let i=0;i<3;i++){const a=(i/3)*Math.PI*2+.5;G.beginPath();G.arc(px+Math.cos(a)*7*s,py+4.5*s+Math.sin(a)*3.5*s,4.5*s,0,Math.PI*2);G.fill();}}
function _broad(s,sw,h){const ht=80*s,tw=8.5*s;G.fillStyle=`hsl(${25+h},44%,17%)`;G.beginPath();G.moveTo(-tw*.5+sw,-ht);G.bezierCurveTo(-tw*.65+sw*.5,-ht*.5,-tw,-ht*.27,-tw,0);G.lineTo(tw,0);G.bezierCurveTo(tw,-ht*.27,tw*.65+sw*.5,-ht*.5,tw*.5+sw,-ht);G.fill();[[52,-ht+4,[114+h,52,16]],[44,-ht-5,[120+h,60,24]],[37,-ht-20,[126+h,66,30]],[29,-ht-31,[130+h,71,37]]].forEach(([r,dy,c])=>{G.fillStyle=`hsl(${c[0]},${c[1]}%,${c[2]}%)`;G.beginPath();G.ellipse(sw*.38,dy,r*s,r*.66*s,0,0,Math.PI*2);G.fill();});}
function _fern(s,sw,h){const ht=62*s;G.strokeStyle=`hsl(${28+h},46%,20%)`;G.lineWidth=4.5*s;G.beginPath();G.moveTo(0,0);G.lineTo(sw,-ht);G.stroke();for(let i=0;i<12;i++){const t2=i/12,a=-Math.PI*.11+t2*Math.PI*1.22-Math.PI*.5,len=(26+Math.sin(i*1.3)*8)*s,lt=26+Math.sin(i*.8)*9,fx=sw+Math.cos(a)*len,fy=-ht+Math.sin(a)*len;G.strokeStyle=`hsl(${118+h+i},62%,${lt}%)`;G.lineWidth=2.6*s;G.beginPath();G.moveTo(sw,-ht);G.quadraticCurveTo(sw+Math.cos(a)*len*.5,(-ht+fy)/2,fx,fy);G.stroke();}}
function _tall(s,sw,h){const ht=112*s;for(let g=0;g<4;g++){G.strokeStyle=`hsl(${30+h+g},44%,${21-g*2}%)`;G.lineWidth=7*s;G.beginPath();G.moveTo(sw*g*.26,-ht*g/4);G.lineTo(sw*(g+1)*.26,-ht*(g+1)/4);G.stroke();}[.35,.56,.73,.9,1.0].forEach((bp,bi)=>{const bx=sw*bp*3.3,by=-ht*bp,bl=(21-bi*3)*s,ls=(14-bi*2)*s;[-1,1].forEach(d=>{G.fillStyle=`hsl(${118+h+bi*3},64%,${32-bi*2}%)`;G.beginPath();G.ellipse(bx+d*bl,by-bl*.36,ls,ls*.72,0,0,Math.PI*2);G.fill();});});}
function _banana(s,sw,h){const ht=76*s;G.fillStyle=`hsl(${85+h},44%,26%)`;G.beginPath();G.moveTo(-6*s,0);G.lineTo(-4.5*s+sw*.5,-ht);G.lineTo(4.5*s+sw*.5,-ht);G.lineTo(6*s,0);G.closePath();G.fill();const bx=sw*.5,by=-ht;[-0.6,-0.2,0.2,0.6,-1.1,1.1,0.0].forEach((la,i)=>{const ll=(52-Math.abs(la)*8)*s,lw=12*s,ex=bx+Math.cos(la-Math.PI/2)*ll,ey=by+Math.sin(la-Math.PI/2)*ll,c1x=bx+Math.cos(la-Math.PI/2)*ll*.3+sw*.14,c1y=by+Math.sin(la-Math.PI/2)*ll*.3-6*s,perp=la+Math.PI/2;G.fillStyle=`hsl(${110+h+i},63%,${24+Math.abs(la)*7}%)`;G.beginPath();G.moveTo(bx,by);G.lineTo(bx+Math.cos(perp)*lw*.5,by+Math.sin(perp)*lw*.5);G.quadraticCurveTo(c1x+Math.cos(perp)*lw*.26,c1y+Math.sin(perp)*lw*.26,ex,ey);G.quadraticCurveTo(c1x-Math.cos(perp)*lw*.26,c1y-Math.sin(perp)*lw*.26,bx-Math.cos(perp)*lw*.5,by-Math.sin(perp)*lw*.5);G.closePath();G.fill();});}
function _mang(s,sw,h){const ht=78*s;for(let r=0;r<7;r++){const rx=(r-3)*10*s+sw*.36,ry=-ht*.36,seed=r*137.5;G.strokeStyle=`hsl(${30+h},44%,20%)`;G.lineWidth=2.2*s;G.beginPath();G.moveTo(rx,ry);G.bezierCurveTo(rx+Math.sin(seed)*5*s,ry+ht*.12,rx+Math.cos(seed)*4*s,ry+ht*.26,rx+Math.sin(seed+1)*3.5*s,0);G.stroke();}G.strokeStyle=`hsl(${28+h},50%,22%)`;G.lineWidth=7*s;G.beginPath();G.moveTo(0,0);G.bezierCurveTo(sw*.5,-ht*.3,sw*.36+3*s,-ht*.57,sw*.72,-ht);G.stroke();[[32,-ht-2,37],[24,-ht-14,43],[18,-ht-24,48]].forEach(([r,dy,lt])=>{G.fillStyle=`hsl(${124+h},70%,${lt}%)`;G.beginPath();G.ellipse(sw*.72,dy,r*s,r*.72*s,0,0,Math.PI*2);G.fill();});}

/* ═══════════ CROWD ═══════════ */
function drawCrowdFigure(c){
  G.save();G.translate(c.x,c.y);
  const armA=Math.sin(c.armAngle)*0.8;
  // Body
  G.fillStyle=c.color;G.beginPath();G.roundRect(-6,0,12,18,3);G.fill();
  // Head
  G.fillStyle=c.skinTone;G.beginPath();G.arc(0,-6,7,0,Math.PI*2);G.fill();
  // Arms waving
  G.strokeStyle=c.skinTone;G.lineWidth=3;G.lineCap='round';
  G.beginPath();G.moveTo(-6,4);G.lineTo(-12+armA*6,4+armA*8);G.stroke();
  G.beginPath();G.moveTo(6,4);G.lineTo(12-armA*6,4-armA*8);G.stroke();
  // Legs
  G.strokeStyle=c.color;G.lineWidth=4;
  G.beginPath();G.moveTo(-3,18);G.lineTo(-3,28);G.stroke();
  G.beginPath();G.moveTo(3,18);G.lineTo(3,28);G.stroke();
  G.restore();
}

/* ═══════════ PLAYER CAR ═══════════ */
function drawCar(){
  const c=car;
  if(c.inv>0&&Math.floor(c.inv/5)%2===1)return;
  const px=c.x,py=c.y,hw=c.w/2,hh=c.h/2;

  // Ghost trail
  if(speed>6.5||boostActive>0){
    const a=Math.min((speed-5)/4,.9)*.12+(boostActive>0?.15:0);
    G.save();G.globalAlpha=a;
    G.fillStyle=`${c.col}55`;
    G.beginPath();G.roundRect(px-hw+7,py-hh,c.w-14,c.h,5);G.fill();
    G.restore();
  }

  // Shield visual
  if(shieldActive>0){
    G.save();
    G.globalAlpha=.3+.2*Math.sin(tickN*.18);
    G.strokeStyle='#44ffff';G.lineWidth=3;
    G.shadowColor='#44ffff';G.shadowBlur=18;
    G.beginPath();G.ellipse(px,py,hw+12,hh+12,0,0,Math.PI*2);G.stroke();
    G.shadowBlur=0;G.restore();
  }

  G.save();G.translate(px,py);G.rotate(c.tilt);
  const hw2=hw,hh2=hh;

  // Shadow
  G.globalAlpha=.28;G.fillStyle='#000';
  G.beginPath();G.ellipse(2,9,hw2*.62,hh2*.25,c.tilt,0,Math.PI*2);G.fill();
  G.globalAlpha=1;

  // Body
  G.fillStyle=c.col;
  G.beginPath();G.roundRect(-hw2,-hh2,c.w,c.h,[6,6,8,8]);G.fill();
  G.fillStyle=c.acc;
  G.beginPath();G.roundRect(-hw2+2,-hh2+2,c.w-4,c.h*.44,[4,4,0,0]);G.fill();
  G.fillStyle=c.dark;
  G.beginPath();G.roundRect(-hw2,-hh2+c.h*.44,c.w,c.h*.56,[0,0,8,8]);G.fill();

  // Hood
  G.fillStyle=c.col;
  G.beginPath();G.roundRect(-hw2+3,-hh2,c.w-6,23,[6,6,0,0]);G.fill();
  G.strokeStyle='rgba(0,0,0,.22)';G.lineWidth=1.5;
  G.beginPath();G.moveTo(-6,-hh2+4);G.lineTo(-6,-hh2+21);G.stroke();
  G.beginPath();G.moveTo(6,-hh2+4);G.lineTo(6,-hh2+21);G.stroke();

  // Windshield
  G.fillStyle='rgba(110,195,255,.78)';
  G.beginPath();G.roundRect(-hw2+8,-hh2+23,c.w-16,20,3);G.fill();
  G.fillStyle='rgba(255,255,255,.22)';
  G.beginPath();G.moveTo(-hw2+10,-hh2+24);G.lineTo(-hw2+18,-hh2+24);G.lineTo(-hw2+10,-hh2+33);G.closePath();G.fill();

  // Cabin
  G.fillStyle='#140403';
  G.beginPath();G.roundRect(-hw2+6,-hh2+22,c.w-12,34,3);G.fill();

  // Driver face
  G.fillStyle='#d4a060';G.beginPath();G.ellipse(-4,-hh2+31,7,8,0,0,Math.PI*2);G.fill();
  G.fillStyle='#1c0e06';
  G.beginPath();G.ellipse(-4,-hh2+25,7.8,5,0,0,Math.PI*2);G.fill();
  G.beginPath();G.ellipse(-12,-hh2+30,3.2,5.5,-.34,0,Math.PI*2);G.fill();
  G.fillStyle='#fff';
  G.beginPath();G.ellipse(-7.5,-hh2+31,2.1,1.6,0,0,Math.PI*2);G.fill();
  G.beginPath();G.ellipse(-1.5,-hh2+31,2.1,1.6,0,0,Math.PI*2);G.fill();
  G.fillStyle='#1a1a1a';
  G.beginPath();G.arc(-7.5,-hh2+31,1.2,0,Math.PI*2);G.fill();
  G.beginPath();G.arc(-1.5,-hh2+31,1.2,0,Math.PI*2);G.fill();
  G.fillStyle='#b87838';G.beginPath();G.arc(-4.5,-hh2+34.5,1.2,0,Math.PI*2);G.fill();
  G.strokeStyle='#7a3618';G.lineWidth=1.3;
  G.beginPath();G.moveTo(-7.5,-hh2+38);G.quadraticCurveTo(-4,-hh2+40,-1,-hh2+38);G.stroke();
  G.strokeStyle='rgba(220,175,0,.52)';G.lineWidth=1.4;
  G.beginPath();G.moveTo(-4,-hh2+24);G.lineTo(hw2-7,-hh2+44);G.stroke();
  G.strokeStyle='#100403';G.lineWidth=2.4;
  G.beginPath();G.arc(-4,-hh2+43,7,0,Math.PI*2);G.stroke();
  G.beginPath();G.moveTo(-4,-hh2+36);G.lineTo(-4,-hh2+43);G.stroke();
  G.beginPath();G.moveTo(-11,-hh2+43);G.lineTo(3,-hh2+43);G.stroke();
  G.fillStyle='#c07050';
  G.beginPath();G.arc(-11,-hh2+43,3,0,Math.PI*2);G.fill();
  G.beginPath();G.arc(3,-hh2+43,3,0,Math.PI*2);G.fill();

  // Rear window
  G.fillStyle='rgba(65,125,200,.5)';
  G.beginPath();G.roundRect(-hw2+9,-hh2+58,c.w-18,14,3);G.fill();

  // Trunk
  G.fillStyle=c.dark;
  G.beginPath();G.roundRect(-hw2+3,hh2-18,c.w-6,12,[0,0,6,6]);G.fill();

  // Door
  G.strokeStyle='rgba(0,0,0,.4)';G.lineWidth=1.1;
  G.beginPath();G.moveTo(0,-hh2+22);G.lineTo(0,hh2-16);G.stroke();
  G.fillStyle='rgba(255,255,255,.22)';
  G.beginPath();G.roundRect(-hw2+4,-5,11,4,2);G.fill();
  G.beginPath();G.roundRect(hw2-15,-5,11,4,2);G.fill();

  // Headlights
  G.shadowColor='#fffcd0';G.shadowBlur=14;G.fillStyle='#fffcd0';
  G.beginPath();G.roundRect(-hw2+4,-hh2+5,14,7,3);G.fill();
  G.beginPath();G.roundRect(hw2-18,-hh2+5,14,7,3);G.fill();
  G.shadowBlur=0;
  G.globalAlpha=.11;G.fillStyle='#ffffc0';
  G.beginPath();G.moveTo(-hw2+11,-hh2+9);G.lineTo(-hw2-15,-hh2-52);G.lineTo(-hw2+25,-hh2-52);G.closePath();G.fill();
  G.beginPath();G.moveTo(hw2-11,-hh2+9);G.lineTo(hw2+15,-hh2-52);G.lineTo(hw2-25,-hh2-52);G.closePath();G.fill();
  G.globalAlpha=1;

  // Taillights
  G.shadowColor='#ff2000';G.shadowBlur=9;G.fillStyle='#ff2000';
  G.beginPath();G.roundRect(-hw2+4,hh2-14,12,7,2);G.fill();
  G.beginPath();G.roundRect(hw2-16,hh2-14,12,7,2);G.fill();
  G.fillStyle='rgba(255,40,0,.26)';G.fillRect(-hw2+17,hh2-13,c.w-34,5);
  G.shadowBlur=0;

  // Boost flame effect
  if(boostActive>0){
    G.save();
    G.globalAlpha=.7+.3*Math.sin(tickN*.3);
    const flame=['#ff4400','#ff8800','#ffcc00'];
    flame.forEach((fc,fi)=>{
      G.fillStyle=fc;
      G.beginPath();
      G.moveTo(-hw2+12+fi*4,hh2);
      G.lineTo(hw2-12-fi*4,hh2);
      G.lineTo(hw2-18-fi*4,hh2+12+fi*6);
      G.lineTo(-hw2+18+fi*4,hh2+12+fi*6);
      G.closePath();G.fill();
    });
    G.restore();
  }

  // Spinning wheels
  const ws=c.wspin;
  [[-hw2,-hh2+9],[hw2-12,-hh2+9],[-hw2,hh2-28],[hw2-12,hh2-28]].forEach(([wx,wy])=>{
    G.fillStyle='#0c0c0c';G.beginPath();G.roundRect(wx,wy,12,20,4);G.fill();
    G.save();G.translate(wx+6,wy+10);G.rotate(ws);
    G.fillStyle='#888';G.beginPath();G.arc(0,0,4.5,0,Math.PI*2);G.fill();
    G.fillStyle='#444';
    for(let sp=0;sp<5;sp++){const sa=(sp/5)*Math.PI*2;G.fillRect(Math.cos(sa)*1.4-.7,Math.sin(sa)*1.4-.7,1.4,1.4);}
    G.fillStyle='#bbb';G.beginPath();G.arc(0,0,2,0,Math.PI*2);G.fill();
    G.restore();
  });

  G.restore(); // ends main car block
}

/* ═══════════ BOSS CAR ═══════════ */
function drawBoss(){
  if(!boss)return;
  const b=boss;
  G.save();G.translate(b.x,b.y);

  // Flashing warning pattern
  G.globalAlpha=.9+.1*Math.sin(tickN*.2);
  G.fillStyle='#111';G.beginPath();G.roundRect(-b.w/2,-b.h/2,b.w,b.h,[4,4,8,8]);G.fill();

  // Police stripes
  for(let s=0;s<6;s++){
    G.fillStyle=s%2===0?'#ff2200':'#220022';
    G.fillRect(-b.w/2+s*(b.w/6),-b.h/2,b.w/6,b.h);
  }
  G.strokeStyle='rgba(0,0,0,.6)';G.lineWidth=2;
  G.beginPath();G.roundRect(-b.w/2,-b.h/2,b.w,b.h,[4,4,8,8]);G.stroke();

  // Police lights
  const lf=Math.floor(tickN/6)%2;
  G.shadowColor=lf?'#ff2200':'#0022ff';G.shadowBlur=20;
  G.fillStyle=lf?'#ff2200':'#2244ff';
  G.beginPath();G.roundRect(-b.w/2+4,-b.h/2+4,18,8,3);G.fill();
  G.shadowColor=lf?'#0022ff':'#ff2200';
  G.fillStyle=lf?'#2244ff':'#ff2200';
  G.beginPath();G.roundRect(b.w/2-22,-b.h/2+4,18,8,3);G.fill();
  G.shadowBlur=0;

  // "BOSS" text
  G.fillStyle='#fff';G.font='bold 10px Rajdhani';G.textAlign='center';
  G.fillText('⚠ BOSS ⚠',0,-b.h/2+18);

  // Wheels
  const bws=(b.wspin||0);
  [[-b.w/2,-b.h/2+8],[b.w/2-12,-b.h/2+8],[-b.w/2,b.h/2-28],[b.w/2-12,b.h/2-28]].forEach(([wx,wy])=>{
    G.fillStyle='#111';G.beginPath();G.roundRect(wx,wy,12,20,4);G.fill();
    G.save();G.translate(wx+6,wy+10);G.rotate(bws);
    G.fillStyle='#888';G.beginPath();G.arc(0,0,4.5,0,Math.PI*2);G.fill();
    G.restore();
  });
  G.restore();
}

/* ═══════════ COINS ═══════════ */
function drawCoin(co){
  G.save();G.translate(co.x,co.y);
  const pulse=.9+.1*Math.sin(tickN*.15+co.phase);
  G.shadowColor='#ffd700';G.shadowBlur=10;
  G.fillStyle='#ffd700';G.beginPath();G.arc(0,0,co.r*pulse,0,Math.PI*2);G.fill();
  G.fillStyle='#ffaa00';G.lineWidth=1.5;G.strokeStyle='#ffaa00';
  G.beginPath();G.arc(0,0,co.r*pulse*.7,0,Math.PI*2);G.stroke();
  G.fillStyle='#fff8';G.font=`bold ${10*pulse}px Rajdhani`;G.textAlign='center';G.textBaseline='middle';
  G.fillText('$',0,0);
  G.shadowBlur=0;G.restore();
}

/* ═══════════ OBSTACLES ═══════════ */
const CLBLS=['MAERSK','EVERGREEN','COSCO','MSC','HAPAG'];
const CCOLS=[{b:'#1a4a8a',s:'#1136a0',t:'#5a9fff'},{b:'#8a1a1a',s:'#a01818',t:'#ff6060'},{b:'#1a7a1a',s:'#18981a',t:'#55ff66'},{b:'#886a18',s:'#a08018',t:'#ffcc55'},{b:'#3a1a7a',s:'#4a1898',t:'#bb55ff'}];
const CPALS=[['#e03020','#a81508','#ff7050'],['#2060d0','#1040a0','#70b0ff'],['#f0b020','#c88010','#ffe070'],['#20a840','#108030','#70e890'],['#ddd','#aaa','#fff'],['#111','#0a0a0a','#555'],['#d06020','#a04010','#ff9060']];
const CTYPES=['sedan','suv','sports','pickup'];

function spawnObs(){
  const lane=Math.floor(rnd()*3);
  const r=rnd();
  const type=r<.28?'car':r<.46?'container':r<.60?'barrel':r<.72?'rock':r<.84?'log':'crate';
  const dm={car:{w:50,h:90},container:{w:58,h:54},barrel:{w:34,h:34},rock:{w:40,h:28},log:{w:80,h:20},crate:{w:42,h:42}};
  obs.push({x:LANES[lane],y:-190,lane,type,...dm[type],col:CCOLS[Math.floor(rnd()*CCOLS.length)],label:CLBLS[Math.floor(rnd()*CLBLS.length)],pal:CPALS[Math.floor(rnd()*CPALS.length)],ct:CTYPES[Math.floor(rnd()*CTYPES.length)],rot:0,wspin:0});
}
function spawnCoin(lane){
  coinObjs.push({x:LANES[lane||Math.floor(rnd()*3)],y:-160,r:10,life:1,phase:rnd()*Math.PI*2,collected:false});
}

function drawObs(o){
  G.save();G.translate(o.x,o.y);
  if(o.type==='car'){
    const hw=o.w/2,hh=o.h/2,[body,dark,acc]=o.pal,ct=o.ct;
    G.globalAlpha=.26;G.fillStyle='#000';G.beginPath();G.ellipse(3,hh+7,hw*.72,8,0,0,Math.PI*2);G.fill();G.globalAlpha=1;
    G.fillStyle=body;
    if(ct==='sports'){G.beginPath();G.moveTo(-hw*.6,-hh);G.lineTo(hw*.6,-hh);G.quadraticCurveTo(hw,-hh,hw,-hh+8);G.lineTo(hw,hh-10);G.quadraticCurveTo(hw,hh,hw*.7,hh);G.lineTo(-hw*.7,hh);G.quadraticCurveTo(-hw,hh,-hw,hh-10);G.lineTo(-hw,-hh+8);G.quadraticCurveTo(-hw,-hh,-hw*.6,-hh);G.fill();}
    else{G.beginPath();G.roundRect(-hw,-hh,o.w,o.h,ct==='suv'?5:4);G.fill();}
    const hH=ct==='pickup'?28:ct==='suv'?25:21;
    G.fillStyle=acc+'bb';G.beginPath();G.roundRect(-hw+3,-hh,o.w-6,hH,[4,4,0,0]);G.fill();
    const wW=o.w-12,wY=-hh+hH,wH=ct==='sports'?14:18;
    G.fillStyle='rgba(130,200,255,.74)';G.beginPath();G.roundRect(-wW/2,wY,wW,wH,3);G.fill();
    const rY=wY+wH,rH=ct==='pickup'?(o.h-hH-wH-26)*.55:o.h-hH-wH-22,rW=ct==='sports'?o.w-14:ct==='suv'?o.w-8:o.w-11;
    G.fillStyle=dark;G.beginPath();G.roundRect(-rW/2,rY,rW,rH,[0,0,3,3]);G.fill();
    G.fillStyle='rgba(65,125,205,.5)';G.beginPath();G.roundRect(-(o.w-14)/2,rY+rH,o.w-14,14,2);G.fill();
    G.shadowColor='#ffffaa';G.shadowBlur=12;G.fillStyle='#fffce0';
    G.beginPath();G.roundRect(-hw+3,-hh+4,12,7,3);G.fill();G.beginPath();G.roundRect(hw-15,-hh+4,12,7,3);G.fill();
    G.fillStyle=acc;G.fillRect(-hw+3,-hh+2,12,3);G.fillRect(hw-15,-hh+2,12,3);G.shadowBlur=0;
    const bi=Math.max(0,Math.min(.18,.04+(640-o.y)/4800));
    if(bi>.005){G.globalAlpha=bi;G.fillStyle='#ffffcc';G.beginPath();G.moveTo(-hw+3,-hh+11);G.lineTo(-hw-20,-hh+88);G.lineTo(-hw+22,-hh+88);G.closePath();G.fill();G.beginPath();G.moveTo(hw-3,-hh+11);G.lineTo(hw+20,-hh+88);G.lineTo(hw-22,-hh+88);G.closePath();G.fill();G.globalAlpha=1;}
    G.shadowColor='#ff2200';G.shadowBlur=9;G.fillStyle='#ff2200';G.beginPath();G.roundRect(-hw+3,hh-10,12,7,2);G.fill();G.beginPath();G.roundRect(hw-15,hh-10,12,7,2);G.fill();G.fillStyle='rgba(255,50,0,.25)';G.fillRect(-hw+17,hh-9,o.w-34,5);G.shadowBlur=0;
    o.wspin=(o.wspin||0)+.14;
    [[-hw-2,-hh+7],[hw-10,-hh+7],[-hw-2,hh-30],[hw-10,hh-30]].forEach(([wx,wy])=>{G.fillStyle='#111';G.beginPath();G.roundRect(wx,wy,12,22,4);G.fill();G.save();G.translate(wx+6,wy+11);G.rotate(o.wspin);G.fillStyle='#888';G.beginPath();G.arc(0,0,4.5,0,Math.PI*2);G.fill();G.fillStyle='#444';for(let sp=0;sp<5;sp++){const sa=(sp/5)*Math.PI*2;G.fillRect(Math.cos(sa)*1.4-.7,Math.sin(sa)*1.4-.7,1.4,1.4);}G.fillStyle='#bbb';G.beginPath();G.arc(0,0,1.9,0,Math.PI*2);G.fill();G.restore();});
  }
  else if(o.type==='container'){const hw=o.w/2,hh=o.h/2,c=o.col;G.globalAlpha=.22;G.fillStyle='#000';G.beginPath();G.ellipse(3,hh+5,hw*.7,7,0,0,Math.PI*2);G.fill();G.globalAlpha=1;G.fillStyle=c.b;G.beginPath();G.roundRect(-hw,-hh,o.w,o.h,4);G.fill();for(let i=0;i<6;i++){G.fillStyle=i%2===0?c.s:c.b;G.fillRect(-hw+(i+1)*o.w/7,-hh,4,o.h);}G.strokeStyle='rgba(0,0,0,.5)';G.lineWidth=1.8;G.beginPath();G.roundRect(-hw,-hh,o.w,o.h,4);G.stroke();G.fillStyle='rgba(0,0,0,.4)';G.fillRect(-hw+5,-8,o.w-10,16);G.fillStyle=c.t;G.font='bold 7.5px Rajdhani';G.textAlign='center';G.fillText(o.label,0,5.5);}
  else if(o.type==='barrel'){G.rotate(o.rot);const r=o.w/2;G.globalAlpha=.2;G.fillStyle='#000';G.beginPath();G.ellipse(2,r+4,r*.72,5,0,0,Math.PI*2);G.fill();G.globalAlpha=1;G.fillStyle='#c04000';G.beginPath();G.arc(0,0,r,0,Math.PI*2);G.fill();G.fillStyle='#882200';G.beginPath();G.arc(0,0,r*.68,0,Math.PI*2);G.fill();[.3,.62,.92].forEach(rf=>{G.strokeStyle='#111';G.lineWidth=3.5;G.beginPath();G.arc(0,0,r*rf,0,Math.PI*2);G.stroke();});G.fillStyle='#ffaa00';G.font='bold 7px Rajdhani';G.textAlign='center';G.fillText('⚠',0,3);}
  else if(o.type==='rock'){G.rotate(o.rot);const rw=o.w/2,rh=o.h/2;G.globalAlpha=.2;G.fillStyle='#000';G.beginPath();G.ellipse(3,rh+4,rw*.72,6,0,0,Math.PI*2);G.fill();G.globalAlpha=1;G.fillStyle='#5a5a5a';G.beginPath();G.moveTo(-rw*.38,-rh);G.lineTo(rw*.58,-rh*.74);G.lineTo(rw,-rh*.17);G.lineTo(rw*.78,rh);G.lineTo(-rw*.5,rh*.88);G.lineTo(-rw,rh*.1);G.lineTo(-rw*.88,-rh*.5);G.closePath();G.fill();G.fillStyle='#787878';G.beginPath();G.moveTo(-rw*.38,-rh);G.lineTo(rw*.58,-rh*.74);G.lineTo(0,-rh*.14);G.closePath();G.fill();}
  else if(o.type==='log'){const hw=o.w/2,hh=o.h/2;G.globalAlpha=.24;G.fillStyle='#000';G.beginPath();G.ellipse(0,hh+4,hw*.82,6,0,0,Math.PI*2);G.fill();G.globalAlpha=1;G.fillStyle='#6a4820';G.beginPath();G.roundRect(-hw,-hh,o.w,o.h,[hh,hh,hh,hh]);G.fill();G.fillStyle='#8a5828';G.beginPath();G.roundRect(-hw+2,-hh+2,o.w-4,hh*.9,[hh,hh,0,0]);G.fill();G.fillStyle='#d4a060';G.beginPath();G.ellipse(-hw,0,4.5,hh,0,0,Math.PI*2);G.fill();G.fillStyle='#6a3818';G.beginPath();G.ellipse(-hw,0,2.2,hh*.6,0,0,Math.PI*2);G.fill();}
  else{const hw=o.w/2,hh=o.h/2;G.globalAlpha=.22;G.fillStyle='#000';G.beginPath();G.ellipse(3,hh+5,hw*.7,6,0,0,Math.PI*2);G.fill();G.globalAlpha=1;G.fillStyle='#8a6228';G.beginPath();G.roundRect(-hw,-hh,o.w,o.h,3);G.fill();G.strokeStyle='#6a4818';G.lineWidth=1.8;G.beginPath();G.moveTo(0,-hh);G.lineTo(0,hh);G.stroke();G.beginPath();G.moveTo(-hw,0);G.lineTo(hw,0);G.stroke();G.strokeStyle='#cc2010';G.lineWidth=2.8;G.beginPath();G.moveTo(-hw+4,-hh+4);G.lineTo(hw-4,hh-4);G.stroke();G.beginPath();G.moveTo(hw-4,-hh+4);G.lineTo(-hw+4,hh-4);G.stroke();}
  G.restore();
}

/* ═══════════ EXPLOSION ═══════════ */
function bigExplosion(x,y){
  // Large fire burst
  for(let i=0;i<35;i++){
    const a=rnd()*Math.PI*2,v=3+rnd()*8;
    const cols=['#ff4400','#ff6600','#ff8800','#ffcc00','#fff'];
    parts.push({x,y,vx:Math.cos(a)*v,vy:Math.sin(a)*v-2,life:1+rnd()*.5,col:cols[Math.floor(rnd()*cols.length)],r:3+rnd()*7,shadow:true,isExh:false});
  }
  // Smoke
  for(let i=0;i<20;i++){
    const a=rnd()*Math.PI*2,v=1+rnd()*3;
    parts.push({x,y:y-10,vx:Math.cos(a)*v,vy:Math.sin(a)*v-3,life:1.5+rnd(),col:'#555',r:5+rnd()*12,shadow:false,isExh:false});
  }
  // Sparks
  for(let i=0;i<15;i++){
    const a=rnd()*Math.PI*2,v=6+rnd()*10;
    parts.push({x,y,vx:Math.cos(a)*v,vy:Math.sin(a)*v-3,life:.6+rnd()*.4,col:'#ffff00',r:1.5+rnd()*2,shadow:true,isExh:false});
  }
}

/* ═══════════ COLLISION ═══════════ */
function doCollision(){
  if(car.inv>0)return;
  const px=car.x-car.w*.82/2,py=car.y-car.h*.84/2;
  const pw=car.w*.82,ph=car.h*.84;
  for(let i=0;i<obs.length;i++){
    const o=obs[i];if(o._hit)continue;
    const ox=o.x-o.w*.42,oy=o.y-o.h*.42,ow=o.w*.84,oh=o.h*.84;
    if(px<ox+ow&&px+pw>ox&&py<oy+oh&&py+ph>oy){
      o._hit=true;
      if(shieldActive>0){shieldActive=0;showPwrToast('🛡️ Shield absorbed hit!');return;}
      lives--;car.inv=95;car.hitFlash=22;
      flashHit();bigExplosion(car.x,car.y-20);updHUD();
      if(lives<=0)endGame();
      return;
    }
  }
  // Boss collision
  if(boss&&car.inv<=0){
    const bx=boss.x-boss.w/2,by=boss.y-boss.h/2;
    if(px<bx+boss.w&&px+pw>bx&&py<by+boss.h&&py+ph>by){
      if(shieldActive>0){shieldActive=0;showPwrToast('🛡️ Shield absorbed boss hit!');return;}
      lives--;car.inv=120;car.hitFlash=30;
      flashHit();bigExplosion(car.x,car.y-10);updHUD();
      if(lives<=0)endGame();
    }
  }
}

function doCoins(){
  for(let i=coinObjs.length-1;i>=0;i--){
    const co=coinObjs[i];if(co.collected)continue;
    const dx=car.x-co.x,dy=car.y-co.y;
    const mag=magnetActive>0;
    if(mag&&Math.abs(dx)<120&&Math.abs(dy)<180){
      co.x+=dx*.08;co.y+=dy*.08;
    }
    if(Math.abs(dx)<20&&Math.abs(dy)<30){
      co.collected=true;sessionCoins++;
      for(let p=0;p<6;p++){const a=rnd()*Math.PI*2;parts.push({x:co.x,y:co.y,vx:Math.cos(a)*3,vy:Math.sin(a)*3,life:.7,col:'#ffd700',r:2+rnd()*3,shadow:true,isExh:false});}
    }
  }
}

/* ═══════════ POWER-UP SPAWNING ═══════════ */
function spawnPowerUp(){
  const types=['shield','magnet','coin_cluster'];
  const t=types[Math.floor(rnd()*types.length)];
  const lane=Math.floor(rnd()*3);
  if(t==='coin_cluster'){
    for(let i=0;i<5;i++)spawnCoin(lane);
  } else {
    obs.push({x:LANES[lane],y:-190,type:'powerup',ptype:t,w:36,h:36,col:{b:'#000',s:'#000',t:'#fff'},label:'',pal:['#000','#000','#000'],ct:'',rot:0,wspin:0});
  }
}

/* ═══════════ DRAW POWER-UP OBSTACLE ═══════════ */
function drawPowerUpObs(o){
  G.save();G.translate(o.x,o.y);
  const r=o.w/2;
  const pulse=.9+.1*Math.sin(tickN*.12);
  const icons={shield:'🛡️',magnet:'🧲'};
  G.shadowColor=o.ptype==='shield'?'#44ffff':'#ff44ff';
  G.shadowBlur=18;
  G.fillStyle=o.ptype==='shield'?'rgba(0,255,255,.2)':'rgba(255,0,255,.2)';
  G.beginPath();G.arc(0,0,r*pulse+4,0,Math.PI*2);G.fill();
  G.fillStyle=o.ptype==='shield'?'#00dddd':'#cc00cc';
  G.beginPath();G.arc(0,0,r*pulse,0,Math.PI*2);G.fill();
  G.shadowBlur=0;
  G.font='18px sans-serif';G.textAlign='center';G.textBaseline='middle';
  G.fillText(icons[o.ptype]||'⭐',0,0);
  G.restore();
}

/* ═══════════ GAME LOOP ═══════════ */
function gameLoop(ts){
  if(gs!=='running'){frameID=null;return;}
  frameID=requestAnimationFrame(gameLoop);
  if(prevTs===0)prevTs=ts;
  const dt=Math.min(ts-prevTs,50);prevTs=ts;accumT+=dt;
  let ticks=0;
  while(accumT>=FDT&&ticks<3){accumT-=FDT;tick();ticks++;}
  if(accumT>FDT*5)accumT=0;
  render();
  drawRain();
}

function tick(){
  tickN++;
  const def=CAR_DEFS[selectedCar];
  const baseSpeed=3.5+Math.pow(score/520,.64)*6.5;
  const boostMult=boostActive>0?1.45:1;
  speed=Math.min(MAXSPD*def.speed,baseSpeed)*boostMult;
  if(K['ArrowUp']||K['KeyW'])speed=Math.min(MAXSPD,speed+.05);
  if(K['ArrowDown']||K['KeyS'])speed=Math.max(3.5,speed-.08);
  score+=speed*.046;roadOff+=speed*.9;

  // Zone change
  if(Math.floor(score/1200)>zoneIdx){
    zoneIdx++;zone=ZONES[zoneIdx%ZONES.length];setZone(zone);skyGrad=null;skyZRef=null;
    if(zone.weather==='rain')initRain();
  }

  // Car
  const cdx=car.targetX-car.x;
  car.x+=cdx*(.28*def.handle);
  if(Math.abs(cdx)<0.4)car.x=car.targetX;
  car.tilt*=0.82;if(Math.abs(car.tilt)<0.002)car.tilt=0;
  car.wspin+=speed*.018*(boostActive>0?1.5:1);
  car.y=GY;

  // Cooldowns
  if(car.inv>0)car.inv--;if(car.hitFlash>0)car.hitFlash--;
  if(shieldActive>0)shieldActive--;if(magnetActive>0)magnetActive--;
  if(boostActive>0)boostActive--;if(boostCooldown>0)boostCooldown--;
  if(lightningTimer>0)lightningTimer--;
  if(zone.weather==='rain'&&tickN%180===0){lightningTimer=8;}

  // Boss logic
  if(score>2000&&!boss&&tickN%600===0&&rnd()<.3){
    boss={x:LANES[1],targetX:car.x,y:-120,w:56,h:100,wspin:0,hp:3};
    bossAlertTimer=180;
    document.getElementById('boss-alert').style.opacity='1';
    setTimeout(()=>document.getElementById('boss-alert').style.opacity='0',2500);
  }
  if(boss){
    boss.wspin+=.12;boss.y+=speed*.4;
    const bdx=car.x-boss.x;boss.x+=bdx*.02;
    if(boss.y>H+160){boss=null;}
  }
  if(bossAlertTimer>0)bossAlertTimer--;

  // Trees
  for(let i=treesL.length-1;i>=0;i--){
    treesL[i].sw+=treesL[i].ss;treesL[i].y+=speed*.64;
    if(treesL[i].y>H+120){const nt=mkTree('L',-130-rnd()*60);treesL.splice(i,1);treesL.unshift(nt);}
  }
  for(let i=treesR.length-1;i>=0;i--){
    treesR[i].sw+=treesR[i].ss;treesR[i].y+=speed*.64;
    if(treesR[i].y>H+120){const nt=mkTree('R',-130-rnd()*60);treesR.splice(i,1);treesR.unshift(nt);}
  }

  // Crowd
  for(let i=crowdL.length-1;i>=0;i--){
    crowdL[i].armAngle+=crowdL[i].waveSpeed;crowdL[i].y+=speed*.45;
    if(crowdL[i].y>H+50){const nc=mkCrowd('L',-80-rnd()*40);crowdL.splice(i,1);crowdL.unshift(nc);}
  }
  for(let i=crowdR.length-1;i>=0;i--){
    crowdR[i].armAngle+=crowdR[i].waveSpeed;crowdR[i].y+=speed*.45;
    if(crowdR[i].y>H+50){const nc=mkCrowd('R',-80-rnd()*40);crowdR.splice(i,1);crowdR.unshift(nc);}
  }

  // Spawn
  spawnClock-=speed;
  if(spawnClock<=0){
    spawnObs();if(rnd()<.3)spawnCoin();
    if(tickN%30===0&&rnd()<.25)spawnPowerUp();
    spawnClock=Math.max(90,180+rnd()*260-Math.min(score/50,110));
  }

  // Move obstacles
  for(let i=obs.length-1;i>=0;i--){
    obs[i].y+=speed;
    if(obs[i].type==='rock'||obs[i].type==='barrel')obs[i].rot+=.014;
    if(obs[i].y>H+170||obs[i]._hit)obs.splice(i,1);
  }

  // Coins
  for(let i=coinObjs.length-1;i>=0;i--){
    coinObjs[i].y+=speed;
    if(coinObjs[i].y>H+50||coinObjs[i].collected)coinObjs.splice(i,1);
  }

  // Power-up collection
  for(let i=obs.length-1;i>=0;i--){
    const o=obs[i];if(o.type!=='powerup'||o._hit)continue;
    if(Math.abs(car.x-o.x)<30&&Math.abs(car.y-o.y)<50){
      o._hit=true;
      if(o.ptype==='shield'){shieldActive=300;showPwrToast('🛡️ Shield Active!');}
      else if(o.ptype==='magnet'){magnetActive=480;showPwrToast('🧲 Magnet Active!');}
    }
  }

  // Particles
  for(let i=parts.length-1;i>=0;i--){
    const p=parts[i];p.x+=p.vx;p.y+=p.vy;p.vy+=.12;p.life-=.024;
    if(p.life<=0)parts.splice(i,1);
  }

  // Exhaust queue (filled in tick, read in render)
  exhaustQ.length=0;
  exhaustQ.push({x:car.x+rndR(-5,5),y:car.y+car.h/2+5,vx:rndR(-.3,.3),vy:-(speed*.3+rnd()*.4),life:.5+rnd()*.25,r:3+rnd()*3});
  for(let i=0;i<obs.length;i++){
    const o=obs[i];
    if(o.type==='car'&&o.y>-20&&o.y<H+30&&tickN%4===0){
      exhaustQ.push({x:o.x+rndR(-5,5),y:o.y+o.h/2+4,vx:rndR(-.3,.3),vy:-(speed*.22+rnd()*.35),life:.44+rnd()*.22,r:3.2+rnd()*3.8});
    }
  }

  // Speed lines
  sLines.length=0;
  if(speed>6.5||boostActive>0){
    const n=Math.min(10,Math.floor((speed-5)*2)+(boostActive>0?4:0));
    for(let i=0;i<n;i++)sLines.push(RL+14+rnd()*RW,rnd()*H,22+rnd()*44);
  }

  doCollision();doCoins();
  if(tickN%10===0)updHUD();
}

function render(){
  G.clearRect(0,0,W,H);drawRoad();

  // Crowd
  for(let i=0;i<crowdL.length;i++)if(crowdL[i].y>0&&crowdL[i].y<H)drawCrowdFigure(crowdL[i]);
  for(let i=0;i<crowdR.length;i++)if(crowdR[i].y>0&&crowdR[i].y<H)drawCrowdFigure(crowdR[i]);

  // Trees
  let li=0,ri=0;
  while(li<treesL.length||ri<treesR.length){
    const lv=li<treesL.length?treesL[li]:null,rv=ri<treesR.length?treesR[ri]:null;
    if(!rv||(lv&&lv.y<=rv.y)){drawTree(lv);li++;}
    else{drawTree(rv);ri++;}
  }

  // Obstacles (insertion sort — no allocation)
  for(let i=1;i<obs.length;i++){const key=obs[i];let j=i-1;while(j>=0&&obs[j].y>key.y){obs[j+1]=obs[j];j--;}obs[j+1]=key;}
  for(let i=0;i<obs.length;i++){
    if(obs[i].type==='powerup')drawPowerUpObs(obs[i]);
    else drawObs(obs[i]);
  }

  // Coins
  for(let i=0;i<coinObjs.length;i++)if(!coinObjs[i].collected)drawCoin(coinObjs[i]);

  // Boss
  drawBoss();

  // Player car
  drawCar();

  // Exhaust
  for(let i=0;i<exhaustQ.length;i++){
    const e=exhaustQ[i];
    G.globalAlpha=e.life*.5;
    G.fillStyle='rgba(155,155,155,.8)';
    G.beginPath();G.arc(e.x,e.y,e.r*e.life,0,Math.PI*2);G.fill();
  }
  G.globalAlpha=1;

  // Particles
  G.shadowBlur=0;
  for(let i=0;i<parts.length;i++){
    const p=parts[i];
    G.globalAlpha=Math.max(0,p.life);
    G.fillStyle=p.col;
    if(p.shadow){G.shadowColor=p.col;G.shadowBlur=8;}
    G.beginPath();G.arc(p.x,p.y,Math.max(.1,p.r*p.life),0,Math.PI*2);G.fill();
    if(p.shadow)G.shadowBlur=0;
  }
  G.globalAlpha=1;G.shadowBlur=0;

  // Speed lines
  if(sLines.length>0){
    const k=Math.min((speed-5)/(MAXSPD-5),1);
    G.globalAlpha=k*.07+(boostActive>0?.06:0);
    G.fillStyle=boostActive>0?'#ffcc44':'#aaff88';
    for(let i=0;i<sLines.length;i+=3)G.fillRect(sLines[i],sLines[i+1],1.5,sLines[i+2]);
    G.globalAlpha=1;
  }

  // Night city neon buildings (background decoration)
  if(isNight){
    G.save();G.globalAlpha=.18;
    const buildings=['#ff00aa','#00aaff','#aa00ff','#ff6600'];
    buildings.forEach((c,i)=>{
      G.fillStyle=c;
      const bx=i<2?i*40:W-RR+i*18,bh=60+i*30;
      G.fillRect(bx,H*.3-bh,22,bh);
      // Windows
      G.fillStyle='rgba(255,255,220,.4)';
      for(let wy=0;wy<bh-10;wy+=8)G.fillRect(bx+3,H*.3-bh+wy+3,6,4);
    });
    G.restore();
  }
}

/* ═══════════ START / END ═══════════ */
function startGame(){
  if(frameID){cancelAnimationFrame(frameID);frameID=null;}
  if(titleID){cancelAnimationFrame(titleID);titleID=null;}
  prevTs=0;accumT=0;tickN=0;
  score=0;lives=3;speed=3.5;roadOff=0;spawnClock=200;
  zoneIdx=0;zone=ZONES[0];skyGrad=null;skyZRef=null;
  shieldActive=0;magnetActive=0;boostCooldown=0;boostActive=0;
  sessionCoins=0;boss=null;bossAlertTimer=0;lightningTimer=0;
  rainDrops=[];RC.style.opacity='0';isNight=false;
  obs.length=0;parts.length=0;sLines.length=0;exhaustQ.length=0;coinObjs.length=0;
  treesL.length=0;treesR.length=0;crowdL.length=0;crowdR.length=0;
  for(let i=0;i<15;i++){treesL.push(mkTree('L',rndR(0,H)));treesR.push(mkTree('R',rndR(0,H)));}
  for(let i=0;i<8;i++){crowdL.push(mkCrowd('L',rndR(0,H)));crowdR.push(mkCrowd('R',rndR(0,H)));}
  treesL.sort((a,b)=>a.y-b.y);treesR.sort((a,b)=>a.y-b.y);
  initCar();updHUD();setZone(zone);rfill();
  gs='running';
  document.getElementById('overlay-main').classList.add('hidden');
  frameID=requestAnimationFrame(gameLoop);
}

function endGame(){
  gs='dead';
  totalCoins+=sessionCoins;
  if(Math.floor(score)>best)best=Math.floor(score);
  highScores.push(Math.floor(score));
  highScores.sort((a,b)=>b-a);
  highScores=highScores.slice(0,10);
  saveData();
  document.getElementById('overlay-main').classList.remove('hidden');
  showScreen('dead');
  document.getElementById('dead-score').textContent=Math.floor(score);
  document.getElementById('dead-best').textContent='BEST: '+best;
  document.getElementById('dead-coins').textContent='🪙 +'+sessionCoins+' coins (Total: '+totalCoins+')';
}

/* ═══════════ SCREENS ═══════════ */
function showScreen(name){
  ['menu','dead','cars','shop','lb'].forEach(s=>{
    document.getElementById('screen-'+s).style.display=s===name?'flex':'none';
  });
}

function buildCarSelector(){
  const grid=document.getElementById('cars-grid');
  grid.innerHTML='';
  document.getElementById('total-coins-cars').textContent='🪙 '+totalCoins+' coins';
  CAR_DEFS.forEach((def,i)=>{
    const owned=ownedCars.includes(i);
    const card=document.createElement('div');
    card.className='car-card'+(i===selectedCar?' selected':'')+(owned?'':' locked');
    const previewCanvas=document.createElement('canvas');
    previewCanvas.width=50;previewCanvas.height=90;
    const pctx=previewCanvas.getContext('2d',{alpha:true});
    drawCarPreview(pctx,def,50,90);
    card.innerHTML=`<div class="car-preview">${def.icon}</div>
      <div class="car-name">${def.name}</div>
      <div class="car-price ${owned?'owned':''}">${owned?'OWNED':'🪙 '+def.price}</div>
      <div class="car-stats">
        <div class="stat-bar-wrap"><div class="stat-label">SPD</div><div class="stat-bar"><div class="stat-fill" style="width:${def.speed*70}%"></div></div></div>
        <div class="stat-bar-wrap"><div class="stat-label">HND</div><div class="stat-bar"><div class="stat-fill" style="width:${def.handle*70}%"></div></div></div>
      </div>`;
    card.addEventListener('click',()=>{
      if(!owned){
        if(totalCoins>=def.price){totalCoins-=def.price;ownedCars.push(i);saveData();}
        else{showPwrToast('Not enough coins!');return;}
      }
      selectedCar=i;saveData();buildCarSelector();
    });
    grid.appendChild(card);
  });
}
function drawCarPreview(ctx,def,w,h){
  // Simple top-down preview
  ctx.fillStyle=def.color;ctx.beginPath();ctx.roundRect(w*.15,h*.05,w*.7,h*.9,[4,4,6,6]);ctx.fill();
  ctx.fillStyle=def.acc;ctx.beginPath();ctx.roundRect(w*.2,h*.07,w*.6,h*.38,3);ctx.fill();
  ctx.fillStyle=def.dark;ctx.beginPath();ctx.roundRect(w*.15,h*.45,w*.7,h*.5,[0,0,6,6]);ctx.fill();
  ctx.fillStyle='rgba(110,195,255,.78)';ctx.beginPath();ctx.roundRect(w*.22,h*.14,w*.56,h*.18,2);ctx.fill();
  ctx.fillStyle='#fffcd0';ctx.fillRect(w*.2,h*.06,w*.18,h*.06);ctx.fillRect(w*.62,h*.06,w*.18,h*.06);
  ctx.fillStyle='#ff2200';ctx.fillRect(w*.2,h*.84,w*.18,h*.06);ctx.fillRect(w*.62,h*.84,w*.18,h*.06);
}

function buildShop(){
  document.getElementById('shop-coins-disp').textContent='🪙 '+totalCoins+' coins';
  const items=[
    {icon:'🛡️',name:'Shield Pack',desc:'Start with 1 shield',price:150,id:'shield_pack'},
    {icon:'🧲',name:'Magnet Pack',desc:'Start with magnet ready',price:200,id:'magnet_pack'},
    {icon:'⚡',name:'Turbo Pack',desc:'Boost cooldown -50%',price:300,id:'turbo_pack'},
    {icon:'❤️',name:'Extra Life',desc:'Start with +1 life',price:400,id:'extra_life'},
  ];
  const container=document.getElementById('shop-items');
  container.innerHTML='';
  items.forEach(item=>{
    const el=document.createElement('div');el.className='shop-item';
    el.innerHTML=`<div class="shop-item-icon">${item.icon}</div><div class="shop-item-info"><div class="shop-item-name">${item.name}</div><div class="shop-item-desc">${item.desc}</div></div><div class="shop-item-price">🪙 ${item.price}</div>`;
    el.addEventListener('click',()=>{
      if(totalCoins>=item.price){totalCoins-=item.price;saveData();buildShop();showPwrToast(item.icon+' Purchased!');}
      else showPwrToast('Not enough coins!');
    });
    container.appendChild(el);
  });
}

function buildLeaderboard(){
  const container=document.getElementById('lb-entries');
  container.innerHTML='';
  const top=highScores.slice(0,8);
  const ranks=['gold','silver','bronze'];
  if(top.length===0){container.innerHTML='<div style="color:rgba(255,255,255,.4);text-align:center;padding:20px;">No scores yet! Play a race.</div>';return;}
  top.forEach((s,i)=>{
    const el=document.createElement('div');
    el.className='lb-item'+(s===Math.floor(score)?' you':'');
    el.innerHTML=`<div class="lb-pos ${ranks[i]||''}">${i===0?'🥇':i===1?'🥈':i===2?'🥉':'#'+(i+1)}</div><div class="lb-entry-name">${i===0?'🏆 Champion':i<3?'Top Racer':'Racer'}</div><div class="lb-entry-score">${Math.floor(s)}</div>`;
    container.appendChild(el);
  });
}

/* ═══════════ NAV ═══════════ */
document.getElementById('btn-play').addEventListener('click',startGame);
document.getElementById('btn-retry').addEventListener('click',startGame);
document.getElementById('btn-menu').addEventListener('click',()=>showScreen('menu'));
document.getElementById('btn-cars').addEventListener('click',()=>{buildCarSelector();showScreen('cars');});
document.getElementById('btn-cars-play').addEventListener('click',startGame);
document.getElementById('btn-cars-back').addEventListener('click',()=>showScreen('menu'));
document.getElementById('btn-shop').addEventListener('click',()=>{buildShop();showScreen('shop');});
document.getElementById('btn-shop-back').addEventListener('click',()=>showScreen('menu'));
document.getElementById('btn-lb').addEventListener('click',()=>{buildLeaderboard();showScreen('lb');});
document.getElementById('btn-lb-back').addEventListener('click',()=>showScreen('menu'));

/* ═══════════ TITLE ANIMATION ═══════════ */
function titleFrame(){
  if(gs!=='menu'){titleID=null;return;}
  titleID=requestAnimationFrame(titleFrame);
  G.fillStyle=zone.sky||'#0d1f08';G.fillRect(0,0,W,H);
  G.fillStyle=zone.side||'#0a1806';G.fillRect(0,0,RL,H);G.fillRect(RR,0,W-RR,H);
  G.fillStyle=zone.road||'#1e1e1e';G.fillRect(RL,0,RW,H);
  for(let i=0;i<treesL.length;i++){treesL[i].sw+=treesL[i].ss;drawTree(treesL[i]);}
  for(let i=0;i<treesR.length;i++){treesR[i].sw+=treesR[i].ss;drawTree(treesR[i]);}
}

/* ═══════════ BOOT ═══════════ */
for(let i=0;i<15;i++){treesL.push(mkTree('L',rndR(0,H)));treesR.push(mkTree('R',rndR(0,H)));}
showScreen('menu');
titleFrame();
</script>
</body>
</html>
