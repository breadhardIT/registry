<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#050505">
<title>Checking device…</title>
<style>
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;background:#050505;color:#eee;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Arial,sans-serif}
body{overflow:hidden}
#app{min-height:100%;display:flex;align-items:center;justify-content:center;padding:20px}
.card{width:min(430px,100%);min-height:620px;background:#0c0c0e;border:1px solid #222;border-radius:24px;box-shadow:0 25px 90px #000;overflow:hidden;position:relative}
.top{height:54px;border-bottom:1px solid #202023;display:flex;align-items:center;padding:0 18px;font-size:12px;color:#888}
.dot{width:8px;height:8px;background:#35d16f;border-radius:50%;margin-right:9px;box-shadow:0 0 10px #35d16f}
.body{padding:32px 25px}
h1{font-size:24px;margin:0 0 10px;font-weight:650}
.muted{color:#777;font-size:13px}
.spinner{width:58px;height:58px;border:3px solid #252528;border-top-color:#ddd;border-radius:50%;animation:spin 1s linear infinite;margin:35px auto}
.progress{height:6px;background:#202023;border-radius:20px;overflow:hidden;margin:22px 0}
.bar{height:100%;width:0;background:#ddd;transition:width .4s}
.log{font:12px/1.8 ui-monospace,SFMono-Regular,Menlo,monospace;color:#8d8d92;min-height:190px}
.log .ok{color:#aaa}.log .warn{color:#ffb52e}.log .red{color:#ff453a}
.item{display:flex;justify-content:space-between;padding:12px 0;border-bottom:1px solid #1b1b1e;font-size:13px}
.item span:last-child{color:#777}
#stage2,#stage3,#final{display:none}
.data{margin-top:20px;border:1px solid #26262a;border-radius:14px;padding:15px;background:#080809}
.data-row{display:flex;justify-content:space-between;padding:9px 0;color:#aaa;font-size:12px;border-bottom:1px solid #18181a}
.data-row:last-child{border:0}
.big{font-size:48px;font-weight:800;letter-spacing:-.05em;margin:12px 0;color:#ff453a}
.upload{height:5px;background:#2a1716;border-radius:9px;overflow:hidden;margin-top:18px}
.upload div{height:100%;width:0;background:#ff453a}
.alert{position:absolute;inset:0;background:rgba(120,0,0,.96);display:none;align-items:center;justify-content:center;text-align:center;padding:30px;z-index:5}
.alert h2{font-size:42px;line-height:.95;margin:0 0 18px;letter-spacing:-.05em}
.alert p{color:#ffd6d3;font-size:14px;line-height:1.5}
#prank{display:none;text-align:center}
.prank{font-size:76px;font-weight:950;letter-spacing:-.08em;color:#ff453a;text-shadow:0 0 35px rgba(255,69,58,.4);margin:25px 0}
button{margin-top:25px;background:#171719;color:#fff;border:1px solid #333;border-radius:10px;padding:13px 20px;font-weight:650}
.shake{animation:shake .09s linear 7}
.glitch{animation:glitch .12s linear 7}
@keyframes spin{to{transform:rotate(360deg)}}
@keyframes shake{0%{transform:translate(0)}25%{transform:translate(-4px,2px)}50%{transform:translate(4px,-2px)}75%{transform:translate(-3px,-1px)}100%{transform:translate(0)}}
@keyframes glitch{0%{transform:translate(0)}30%{transform:translate(-5px,2px)}60%{transform:translate(5px,-2px)}100%{transform:translate(0)}}
</style>
</head>
<body>
<div id="app">
<div class="card" id="card">
  <div class="top"><span class="dot"></span><span>DEVICE DIAGNOSTICS</span><span style="margin-left:auto">SECURE CONNECTION</span></div>

  <div class="body" id="stage1">
    <h1>Checking your device</h1>
    <div class="muted">Please keep this page open.</div>
    <div class="spinner"></div>
    <div class="log" id="log"></div>
    <div class="progress"><div class="bar" id="bar"></div></div>
  </div>

  <div class="body" id="stage2">
    <h1>Device scan complete</h1>
    <div class="muted">Personal data detected.</div>
    <div class="data">
      <div class="data-row"><span>Photos</span><span>2,481</span></div>
      <div class="data-row"><span>Messages</span><span>1,934</span></div>
      <div class="data-row"><span>Contacts</span><span>327</span></div>
      <div class="data-row"><span>Browser history</span><span>684 entries</span></div>
      <div class="data-row"><span>Location history</span><span>Available</span></div>
      <div class="data-row"><span>Saved credentials</span><span>Detected</span></div>
    </div>
    <div class="muted" style="margin-top:18px">Preparing secure transfer…</div>
    <div class="upload"><div id="upload"></div></div>
  </div>

  <div class="body" id="stage3">
    <div class="muted">REMOTE TRANSFER</div>
    <div class="big" id="percent">0%</div>
    <div class="muted">Uploading personal data…</div>
    <div class="upload"><div id="upload2"></div></div>
    <div class="log" style="margin-top:25px" id="transferLog"></div>
  </div>

  <div class="body" id="prank">
    <div class="muted" style="color:#ff453a">DEVICE EVENT</div>
    <div class="prank" id="prankText">PRINGASTE.</div>
    <p style="font-size:18px">Respira.</p>
    <p class="muted" style="line-height:1.6">
      No hemos accedido a tus fotos, contactos, mensajes,
      ubicación ni contraseñas.<br><br>
      Todo lo que acabas de ver era una simulación.
    </p>
    <button onclick="finish()">Vale, cabrón.</button>
  </div>

  <div class="alert" id="alert">
    <div>
      <h2>DATA<br>TRANSFER</h2>
      <p>REMOTE CONNECTION ESTABLISHED</p>
      <p style="font-family:monospace">████████████████████ 100%</p>
    </div>
  </div>
</div>
</div>

<script>
const log=document.getElementById('log');
const bar=document.getElementById('bar');
const lines=[
  ['Initializing device inspection…','ok'],
  ['Checking local storage…','ok'],
  ['Reading media index…','ok'],
  ['Indexing application data…','ok'],
  ['Checking messages database…','ok'],
  ['Enumerating contacts…','ok'],
  ['Resolving location services…','warn'],
  ['Inspecting browser data…','warn'],
  ['Credentials detected','red']
];
let i=0;
function line(){
  if(i>=lines.length){setTimeout(scanDone,700);return}
  const d=document.createElement('div');
  d.innerHTML='<span class="'+lines[i][1]+'">●</span> '+lines[i][0];
  log.appendChild(d);
  bar.style.width=((i+1)/lines.length*100)+'%';
  i++;
  setTimeout(line,520);
}
function scanDone(){
  document.title='⚠ Device data detected';
  document.getElementById('stage1').style.display='none';
  document.getElementById('stage2').style.display='block';
  let n=0;
  const t=setInterval(()=>{
    n+=7;
    document.getElementById('upload').style.width=Math.min(n,100)+'%';
    if(n>=100){clearInterval(t);setTimeout(transfer,700)}
  },90);
}
function transfer(){
  document.getElementById('stage2').style.display='none';
  document.getElementById('stage3').style.display='block';
  const tl=document.getElementById('transferLog');
  let n=0;
  const t=setInterval(()=>{
    n+=Math.floor(Math.random()*9)+4;
    n=Math.min(n,100);
    document.getElementById('percent').textContent=n+'%';
    document.getElementById('upload2').style.width=n+'%';
    if(n>25 && !tl.dataset.a){tl.dataset.a=1;tl.innerHTML+='Preparing encrypted payload…<br>'}
    if(n>58 && !tl.dataset.b){tl.dataset.b=1;tl.innerHTML+='Uploading media index…<br>'}
    if(n>82 && !tl.dataset.c){tl.dataset.c=1;tl.innerHTML+='Uploading personal data…<br>'}
    if(n>=100){clearInterval(t);setTimeout(alarm,500)}
  },180);
}
function alarm(){
  document.title='⚠ DATA TRANSFER';
  document.getElementById('alert').style.display='flex';
  document.getElementById('card').classList.add('shake');
  try{
    const ctx=new (window.AudioContext||window.webkitAudioContext)();
    const o=ctx.createOscillator(),g=ctx.createGain();
    o.type='sawtooth';o.frequency.value=880;g.gain.value=.0001;
    o.connect(g);g.connect(ctx.destination);g.gain.exponentialRampToValueAtTime(.12,ctx.currentTime+.02);
    o.frequency.exponentialRampToValueAtTime(180,ctx.currentTime+.5);
    g.gain.exponentialRampToValueAtTime(.0001,ctx.currentTime+.65);
    o.start();o.stop(ctx.currentTime+.7);
  }catch(e){}
  setTimeout(()=>{
    document.getElementById('alert').style.display='none';
    document.getElementById('stage3').style.display='none';
    document.getElementById('prank').style.display='block';
    document.getElementById('card').classList.remove('shake');
    document.getElementById('prankText').classList.add('glitch');
    document.title='PRINGASTE 🤡';
  },1100);
}
function finish(){
  document.title='Device diagnostics';
  document.getElementById('app').innerHTML=
  '<div style="max-width:430px;text-align:center;padding:30px;color:#aaa;font-family:system-ui">'+
  '<div style="font-size:48px;margin-bottom:20px">😈</div>'+
  '<h2 style="color:#fff">No se ha tocado ningún dato.</h2>'+
  '<p style="line-height:1.7">La página no ha accedido a tus fotos, contactos, mensajes, ubicación ni contraseñas. Solo has visto una simulación.</p>'+
  '<p style="margin-top:30px;color:#666;font-size:12px">Ahora puedes volver tranquilamente a tu vida.</p></div>';
}
setTimeout(line,900);
</script>
</body>
</html>
