<!doctype html>  
<html lang="ja">  
<head>  
  <meta charset="utf-8" />  
  <meta name="viewport" content="width=device-width,initial-scale=1" />  
  <meta name="apple-mobile-web-app-capable" content="yes">  
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">  
  <meta name="apple-mobile-web-app-title" content="Bluetooth Connect Demo">  
  <link rel="apple-touch-icon" href="https://upload.wikimedia.org/wikipedia/commons/2/23/Bluetooth_icon.svg">  
  <title>Bluetooth Connect Demo</title>  
  <style>  
    :root{font-family:-apple-system,BlinkMacSystemFont,"Helvetica Neue",Arial; color:#111;}  
    body{margin:0;padding:20px;background:#f4f6f8;display:flex;align-items:center;justify-content:center;min-height:100vh;}  
    .card{background:#fff;border-radius:14px;box-shadow:0 6px 20px rgba(0,0,0,0.08);width:min(980px,96vw);padding:20px;box-sizing:border-box;}  
    header{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;}  
    header h1{font-size:20px;margin:0;}  
    .note{font-size:13px;color:#666;margin-top:6px;}  
    .controls{display:flex;gap:10px;flex-wrap:wrap;margin-top:10px;}  
    button{padding:12px 16px;border-radius:10px;border:1px solid #ddd;background:linear-gradient(#fff,#fafafa);font-size:16px;cursor:pointer;}  
    button:disabled{opacity:0.5;cursor:default;}  
    #devices{margin-top:16px;display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px;}  
    .device{border:1px solid #eee;padding:12px;border-radius:10px;background:#fbfbff;}  
    .device h3{margin:0 0 6px 0;font-size:15px;}  
    .status{font-size:13px;color:#555;margin-bottom:8px;}  
    #log{margin-top:14px;background:#0f1720;color:#e6ffed;padding:10px;border-radius:8px;min-height:120px;overflow:auto;font-family:monospace;font-size:13px;}  
    .warning{background:#fff4e5;border-left:4px solid #ffb020;padding:10px;border-radius:8px;margin-top:12px;color:#663f00;}  
    .success{background:#ecffef;border-left:4px solid #28a745;padding:10px;border-radius:8px;margin-top:12px;color:#0b6830;}  
    .hint{font-size:13px;color:#3a3a3a}  
    .small{font-size:13px;color:#666}  
    footer{margin-top:14px;font-size:13px;color:#777;text-align:right}  
    .row{display:flex;align-items:center;gap:8px}  
    #pwaTip{background:#e9f5ff;border-left:4px solid #007aff;padding:10px;border-radius:8px;margin-top:14px;color:#003e7e;display:none;}  
  </style>  
</head>  
<body>  
  <div class="card" role="main">  
    <header>  
      <div>  
        <h1>Bluetooth Connect Demo</h1>  
        <div class="note">Bluefy / BLE Link **向け** iPad Web Bluetooth **デモ**</div>  
      </div>  
      <div class="row">  
        <div id="availBadge" class="small">Bluetooth: **調査中**…</div>  
      </div>  
    </header>  
  
    <div id="unsupported" class="warning" style="display:none;">  
      <strong>**注意：**</strong> Safari**では** Web Bluetooth **がサポートされていません。**<br>  
      <span class="hint">App Store **の「**Bluefy**」または「**BLE Link**」でこのページを開いてください。**</span>  
    </div>  
  
    <div class="controls" aria-hidden="false">  
      <button id="checkBtn">**対応確認**</button>  
      <button id="scanBtn" disabled>**デバイスをスキャン**</button>  
      <button id="reconnectBtn" disabled>**再接続**</button>  
      <button id="disconnectAllBtn" disabled>**全切断**</button>  
    </div>  
  
    <div id="devices" aria-live="polite"></div>  
    <div id="log" aria-live="polite">**ログ**:</div>  
  
    <div id="pwaTip">  
      📱 **ホーム画面に追加するとアプリのように起動できます！**<br>  
      Bluefy**右上の共有メニュー** → **「ホーム画面に追加」を選んでください。**  
    </div>  
  
    <footer>Bluetooth Connect Demo — iPad**対応** Bluefy**版**</footer>  
  </div>  
  
<script>  
const logEl=document.getElementById('log');const scanBtn=document.getElementById('scanBtn');  
const reconnectBtn=document.getElementById('reconnectBtn');const disconnectAllBtn=document.getElementById('disconnectAllBtn');  
const unsupported=document.getElementById('unsupported');const availBadge=document.getElementById('availBadge');  
const checkBtn=document.getElementById('checkBtn');const devicesDiv=document.getElementById('devices');  
function log(...args){logEl.textContent+="\n"+args.join(" ");logEl.scrollTop=logEl.scrollHeight;}  
function supportsWebBluetooth(){return navigator&&'bluetooth'in navigator;}  
async function updateAvailability(){if(!supportsWebBluetooth()){availBadge.textContent='**未サポート**';unsupported.style.display='block';scanBtn.disabled=true;return;}  
try{if(navigator.bluetooth.getAvailability){const av=await navigator.bluetooth.getAvailability();availBadge.textContent=av?'**利用可能**':'**制限中**';scanBtn.disabled=!av;unsupported.style.display=av?'none':'block';}else{availBadge.textContent='API**あり**';scanBtn.disabled=false;unsupported.style.display='none';}}catch(e){availBadge.textContent='**取得エラー**';scanBtn.disabled=true;log(e);} }  
checkBtn.addEventListener('click',()=>{updateAvailability();log('Bluetooth**確認**')});  
scanBtn.addEventListener('click',async()=>{try{const dev=await navigator.bluetooth.requestDevice({acceptAllDevices:true,optionalServices:**[**'battery_service'**]**});  
log('**選択**:'+dev.name);alert('**デバイス選択**:'+dev.name);}catch(e){log('**キャンセル**:'+ (e && e.message ? e.message : e))}});  
updateAvailability();log('Bluetooth Connect Demo Bluefy**版** **初期化**');  
// Show PWA tip if standalone mode not active  
if(!window.matchMedia('(display-mode: standalone)').matches){document.getElementById('pwaTip').style.display='block';}  
</script>  
</body>  
</html>  
