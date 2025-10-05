<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SkyBet Aviator — UFO Edition</title>
<style>
:root {
  --bg1: #030417; --yellow: #ffc107; --green: #4be37a; --red: #ff4b6e; --pink: #ff66cc; --muted: #9aa;
}
body{margin:0;font-family:Arial,sans-serif;background:var(--bg1);color:#fff;display:flex;justify-content:center;align-items:flex-start;height:100vh;overflow-y:auto;padding:12px;}
.container{position:relative;width:100%;max-width:1000px;z-index:1;}
.top{display:flex;justify-content:center;align-items:center;flex-direction:column;padding:12px 20px;background: rgba(255,255,255,0.05);border-radius: 14px;text-align:center;margin-bottom:8px;}
.top-center .line1{display:flex;justify-content:center;gap:20px;font-weight:700;font-size:15px;margin:5px 0;align-items:center; flex-wrap:wrap;}
.top-center .line1 img{width:20px;height:20px;margin-right:4px;}
.top-center .line2 {display:flex; gap:8px; width:100%; max-width:600px;}
.top-center .line2 button {
  flex: 1;
  padding: 8px 12px;
  font-size: 14px;
  border-radius: 8px;
  border: none;
  background: var(--pink);
  color: #fff;
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.2s, background 0.2s;
}
.top-center .line2 button:hover {
  transform: scale(1.05);
  background: #ff85c1;
}
.brand{font-weight:900;font-size:20px;animation:brandGlow 4s infinite;}
@keyframes brandGlow {0%{color:#ffc107;text-shadow:0 0 6px #ffc107;}25%{color:#ff85c1;text-shadow:0 0 10px #ff85c1;}50%{color:#4be37a;text-shadow:0 0 10px #4be37a;}75%{color:#ffc107;text-shadow:0 0 10px #ffc107;}100%{color:#ffc107;text-shadow:0 0 6px #ffc107;}}
.play-area{display:flex;gap:12px;flex-wrap:wrap;position:relative;}
.left{flex:1;display:flex;flex-direction:column;align-items:center;}
.panel{background:rgba(255,255,255,0.02);padding:12px;border-radius:10px;width:100%;position:relative;}
.game-wrap{position:relative;height:460px;border-radius:12px;overflow:hidden;background:radial-gradient(circle at 50% 12%, rgba(6,16,34,0.9) 0%, #000 70%);border:1px solid rgba(255,255,255,0.02);}
canvas{position:absolute;inset:0;width:100%;height:100%;pointer-events:none;z-index:1;}
#planeWrap{position:absolute;left:0;bottom:0;z-index:2;transform-origin:center;will-change:transform;}
.multDisplay{position:absolute;left:50%;top:15%;transform:translateX(-50%);font-size:48px;font-weight:900;z-index:3;white-space:nowrap;text-align:center;color:#ffc107;}
.winAnimation{color:#4be37a;text-shadow:0 0 12px #4be37a,0 0 24px #0ff;animation: popBounce 1s ease forwards;}
@keyframes popBounce{0%{transform: translateX(-50%) scale(0.5);opacity:0;}50%{transform: translateX(-50%) scale(1.3);opacity:1;}100%{transform: translateX(-50%) scale(1);opacity:1;}}
#placeBetBtn{width:100%;padding:14px;font-size:20px;border-radius:12px;border:none;font-weight:700;color:#111;cursor:pointer;margin-top:10px;transition:0.2s;}
.footer{font-size:12px;color:var(--muted);text-align:center;margin-top:6px;}
.modal{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);background:#0e1320;padding:20px;border-radius:12px;width:320px;box-shadow:0 0 20px rgba(255,255,255,0.2);z-index:9999;display:none;}
.modal input{width:100%;padding:10px;margin:6px 0;border-radius:8px;border:none;background:#111;color:#fff;}
.modal button{width:100%;padding:12px;margin-top:10px;border:none;border-radius:10px;background:#4be37a;color:#111;font-weight:700;cursor:pointer;}
.modal button.closeBtn{background:#ff4b6e;margin-top:6px;}
.winners{margin-top:12px;padding:12px;background:rgba(255,255,255,0.05);border-radius:12px;text-align:center;}
.winners h3{margin-bottom:8px;color:#ffc107;}
.winner-list{display:flex;flex-direction:column;gap:4px;}
</style>
</head>
<body>

<div class="container" id="gameScreen">
  <div class="top">
    <div class="top-center">
      <div class="line1">
        <span class="brand">SkyBet — Aviator</span> |
        <span><img src="https://img.icons8.com/emoji/24/ffffff/coin-emoji.png" alt="Coins"> 
          Coins: <strong id="coinsDisplay">1000.00</strong>
        </span> |
        <span>Deposit: <strong id="depositDisplay">0 LKR</strong></span> |
        <span>Daily Chances: <strong id="dailyChancesDisplay">0</strong></span>
      </div>
      <div class="line2" id="topButtons">
        <button id="flightPackBtn">Flight Pack</button>
        <button id="authBtn">Login</button>
        <button id="musicBtn">🎵</button>
        <button id="adminBtn" style="display:none;">Admin</button>
      </div>
    </div>
  </div>

  <div class="play-area">
    <div class="left">
      <div class="game-wrap panel" id="gameWrap">
        <canvas id="stars"></canvas>
        <div id="planeWrap">
          <svg width="120" height="120" viewBox="0 0 500 300" xmlns="http://www.w3.org/2000/svg">
            <polygon id="ufoBeam" points="220,120 280,120 350,300 150,300" fill="rgba(0,255,255,0.4)" style="opacity:0.5;">
              <animate attributeName="opacity" values="0;0.6;0" dur="2s" repeatCount="indefinite"></animate>
            </polygon>
            <ellipse cx="250" cy="120" rx="100" ry="30" fill="#aaa" stroke="#222" stroke-width="3"></ellipse>
            <ellipse cx="250" cy="115" rx="60" ry="18" fill="#777" stroke="#222" stroke-width="2"></ellipse>
            <ellipse cx="250" cy="100" rx="40" ry="20" fill="url(#ufoDomeGrad)"></ellipse>
            <defs>
              <radialGradient id="ufoDomeGrad" cx="50%" cy="50%" r="50%">
                <stop offset="0%" stop-color="#0f0" stop-opacity="0.8"></stop>
                <stop offset="100%" stop-color="#004400" stop-opacity="0.5"></stop>
              </radialGradient>
            </defs>
          </svg>
        </div>
        <div class="multDisplay" id="multDisplay">Try Your Luck!</div>
      </div>
      <div class="panel" style="margin-top:12px;width:80%;text-align:center;">
        <button id="placeBetBtn">Start</button>
      </div>
    </div>
  </div>

  <div class="winners">
    <h3>🏆 Recent Winners</h3>
    <div class="winner-list" id="winnerList">
      <span>No winners yet</span>
    </div>
  </div>

  <div class="footer">SkyBet Aviator — Demo</div>
</div>

<!-- Unified Auth Modal -->
<div class="modal" id="authModal">
  <h3 id="authTitle">Login</h3>
  <div id="loginForm">
    <input id="authMobile" type="text" placeholder="Mobile No">
    <input id="authPassword" type="password" placeholder="Password">
    <button onclick="handleLogin()">Login</button>
    <p style="margin-top:6px; cursor:pointer; color:#ffc107;" onclick="showForgot()">Forgot Password?</p>
    <p style="margin-top:6px; cursor:pointer; color:#4be37a;" onclick="showSignUp()">Sign Up</p>
  </div>
  <div id="signUpForm" style="display:none;">
    <input id="signUpMobile" type="text" placeholder="Mobile No">
    <input id="signUpPassword" type="password" placeholder="Password">
    <input id="signUpEmail" type="email" placeholder="Email (optional)">
    <button onclick="handleSignUp()">Sign Up</button>
    <p style="margin-top:6px; cursor:pointer; color:#ffc107;" onclick="showLogin()">Back to Login</p>
  </div>
  <div id="forgotForm" style="display:none;">
    <input id="forgotMobile" type="text" placeholder="Mobile No">
    <button onclick="handleForgot()">Reset Password</button>
    <p style="margin-top:6px; cursor:pointer; color:#ffc107;" onclick="showLogin()">Back to Login</p>
  </div>
  <button class="closeBtn" onclick="closeModal('authModal')">Close</button>
</div>

<!-- Flight Pack Modal -->
<div class="modal" id="flightModal">
  <h3>Flight Pack</h3>
  <p>Deposit: <strong>500 LKR</strong></p>
  <p>Daily Chances: <strong>100</strong></p>
  <p>Countdown: <span id="flightCountdown">30:00:00</span></p>
  <p>Bank Details:</p>
  <ul>
    <li>Bank: ABC Bank</li>
    <li>Account No: 123456789</li>
    <li>Upload Slip:</li>
    <input type="file" id="slipUpload">
  </ul>
  <button onclick="closeModal('flightModal')">Close</button>
</div>

<!-- Admin Modal -->
<div class="modal" id="adminModal">
  <h3>Admin Panel</h3>
  <p>Manage users and deposits</p>
  <button onclick="closeModal('adminModal')">Close</button>
</div>

<!-- Background Music -->
<audio id="bgMusic" loop>
  <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
</audio>

<script src="https://unpkg.com/parse/dist/parse.min.js"></script>
<script>
Parse.initialize("zG9RNDK8qvLIh7oqHDAXKuL0CsrOqQrpFhFJcbIB","H1e3gUf6X3oyAr41f9VUN1MGI5mIYfO3Cr6NgoO9");
Parse.serverURL = "https://parseapi.back4app.com";

const authBtn = document.getElementById('authBtn');
const flightPackBtn = document.getElementById('flightPackBtn');
const adminBtn = document.getElementById('adminBtn');
const placeBetBtn = document.getElementById('placeBetBtn');
const coinsDisplay = document.getElementById('coinsDisplay');
const multDisplay = document.getElementById('multDisplay');
const planeWrap = document.getElementById('planeWrap');
const musicBtn = document.getElementById('musicBtn');
const bgMusic = document.getElementById('bgMusic');
let isLoggedIn=false, balance=1000, roundMultiplier=1, crashPoint=0, flightRaf=null, isMusicPlaying=false;
let isAdmin=false;

// Modal functions
function openModal(id){ document.getElementById(id).style.display='block'; }
function closeModal(id){ document.getElementById(id).style.display='none'; }
function showLogin(){ document.getElementById("authTitle").innerText="Login"; document.getElementById("loginForm").style.display="block"; document.getElementById("signUpForm").style.display="none"; document.getElementById("forgotForm").style.display="none";}
function showSignUp(){ document.getElementById("authTitle").innerText="Sign Up"; document.getElementById("loginForm").style.display="none"; document.getElementById("signUpForm").style.display="block"; document.getElementById("forgotForm").style.display="none";}
function showForgot(){ document.getElementById("authTitle").innerText="Forgot Password"; document.getElementById("loginForm").style.display="none"; document.getElementById("signUpForm").style.display="none"; document.getElementById("forgotForm").style.display="block";}

// Auth button
authBtn.addEventListener('click',()=>{ openModal('authModal'); showLogin(); });
// Flight Pack button
flightPackBtn.addEventListener('click',()=>{ 
  if(!isLoggedIn){ openModal('authModal'); } 
  else { openModal('flightModal'); startCountdown(); } 
});
// Admin button
adminBtn.addEventListener('click',()=>{ openModal('adminModal'); });

// Music toggle
musicBtn.addEventListener('click', ()=>{
  if(isMusicPlaying){ bgMusic.pause(); isMusicPlaying=false; musicBtn.innerText='🎵'; }
  else { bgMusic.play(); isMusicPlaying=true; musicBtn.innerText='🔊'; }
});

// Countdown example
let countdownInterval=null;
function startCountdown(){
  clearInterval(countdownInterval);
  let totalSeconds=30*60*60;
  const display=document.getElementById('flightCountdown');
  countdownInterval=setInterval(()=>{
    const h=Math.floor(totalSeconds/3600); const m=Math.floor((totalSeconds%3600)/60); const s=totalSeconds%60;
    display.innerText=`${h.toString().padStart(2,'0')}:${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
    if(totalSeconds>0) totalSeconds--; else clearInterval(countdownInterval);
  },1000);
}

// Handle login with admin check
async function handleLogin(){
  const mobile=document.getElementById("authMobile").value.trim();
  const password=document.getElementById("authPassword").value.trim();
  if(!mobile||!password){ alert("Required fields!"); return; }
  if(mobile==="admin" && password==="20030218"){ // Admin credentials
    isAdmin=true; isLoggedIn=true; adminBtn.style.display="inline-block"; authBtn.style.display="none"; closeModal('authModal'); alert("Admin logged in"); return;
  }
  try{
    const user=await Parse.User.logIn(mobile,password);
    isLoggedIn=true;
    balance=parseFloat(user.get("balance")||1000);
    coinsDisplay.innerText=balance.toFixed(2);
    authBtn.innerText="Logout"; closeModal('authModal');
  }catch(e){ alert("Invalid credentials"); }
}

// Logout
authBtn.addEventListener('click', ()=>{
  if(isLoggedIn && !isAdmin){ Parse.User.logOut(); isLoggedIn=false; authBtn.innerText="Login"; alert("Logged out"); }
});

// Placeholder SignUp / Forgot (similar to previous)
function handleSignUp(){ alert("SignUp demo"); }
function handleForgot(){ alert("Password reset link sent"); }

// Background stars
const starsCanvas=document.getElementById('stars');
const starsCtx=starsCanvas.getContext('2d');
function fitCanvas(){ starsCanvas.width=starsCanvas.parentElement.clientWidth; starsCanvas.height=starsCanvas.parentElement.clientHeight; }
fitCanvas(); window.addEventListener('resize',fitCanvas);
const stars=Array.from({length:100},()=>({x:Math.random()*starsCanvas.width,y:Math.random()*starsCanvas.height,r:Math.random()*1.5+0.5,speed:Math.random()*0.3+0.1}));
function drawStars(){ starsCtx.clearRect(0,0,starsCanvas.width,starsCanvas.height); for(const s of stars){ starsCtx.beginPath(); starsCtx.arc(s.x,s.y,s.r,0,Math.PI*2); starsCtx.fillStyle='rgba(255,255,255,0.7)'; starsCtx.fill(); s.x-=s.speed; if(s.x<0) s.x=starsCanvas.width; } requestAnimationFrame(drawStars);}
drawStars();
</script>
</body>
</html># Skybet