<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Cloud Game Vip</title>
  <style>
    * {
      box-sizing: border-box;
      transition: all 0.3s ease;
    }
    body, html {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
      background: pink;
      transition: background 0.5s ease, color 0.3s ease;
    }
    header {
      background: #87CEEB;
      color: red;
      padding: 15px;
      font-size: 24px;
      text-align: center;
      position: relative;
    }
    #clock {
      position: absolute;
      right: 15px;
      top: 15px;
      font-size: 14px;
    }
    .content, #settingPanel, #helpPanel, #vpnPanel {
      padding: 20px;
      display: none;
      animation: fadeInSlide 0.4s ease;
    }
    .device {
      margin: 10px 0;
      background: purple;
      padding: 12px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }
    .btn {
      background:#FF69B4;
      color: #FFD700;
      border: none;
      padding: 8px 14px;
      border-radius: 6px;
      cursor: pointer;
      margin: 6px 4px;
      transition: transform 0.2s ease;
    }
    .btn:active {
      transform: scale(0.95);
    }
    .hidden { display: none; }
    .tabbar {
      position: fixed;
      bottom: 0;
      width: 100%;
      display: flex;
      justify-content: space-around;
      padding: 10px 0;
      background: #87CEEB;
    }
    #iphoneView {
      display: none;
      position: fixed;
      inset: 0;
      background: #000;
      z-index: 9999;
      padding: 10px;
    }
    iframe {
      width: 100%;
      height: calc(100% - 50px);
      border: none;
      border-radius: 14px;
    }
    .bg-sample {
      width: 60px;
      height: 40px;
      border: 2px solid #ccc;
      margin: 4px;
      cursor: pointer;
      border-radius: 6px;
      background-size: cover;
      background-position: center;
    }
    .bg-container {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }
    body.dark {
      background: #111;
      color: white;
    }
    body.dark header {
      background: #222;
      color: white;
    }
    body.dark .tabbar {
      background: #333;
    }
    @keyframes fadeInSlide {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .device-slider {
      display: flex;
      overflow-x: auto;
      gap: 12px;
      padding: 10px 0;
    }
    .device-card {
      min-width: 220px;
      height: 420px;
      background: none;
      border-radius: 24px;
      border: 3px solid #444;
      padding: 10px;
      position: relative;
      color: white;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      box-shadow: 0 0 8px rgba(0, 0, 0, 0.6);
    }
    .device-title {
      text-align: center;
      font-weight: bold;
      font-size: 16px;
      margin-top: 10px;
    }
    .device-time {
      font-size: 12px;
      text-align: center;
      margin-bottom: 8px;
      opacity: 0.7;
    }
    .device-actions {
      display: flex;
      justify-content: space-around;
      margin-bottom: 12px;
    }
  </style>
</head>
<body>
<script>
const STORAGE_KEY = "cloudgame_accounts";
const SESSION_KEY = "cloudgame_session";

// Xử lý đăng nhập
function handleLogin() {
  const user = document.getElementById("username").value.trim();
  const pass = document.getElementById("password").value;
  const users = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {};

  if (!user || !pass) {
    document.getElementById("loginMessage").innerText = "Vui lòng nhập đầy đủ thông tin!";
    return;
  }

  if (users[user] && users[user].password === pass) {
    localStorage.setItem(SESSION_KEY, JSON.stringify({ username: user }));
    localStorage.setItem('userData', JSON.stringify({ username: user }));
    document.getElementById("loginScreen").style.display = "none";
    document.getElementById("mainContent").style.display = "block";
    updateAccountInfoUI();
  } else {
    document.getElementById("loginMessage").innerText = "Sai tài khoản hoặc mật khẩu!";
  }
}

// Xử lý đăng ký
function handleRegister() {
  const user = document.getElementById("username").value.trim();
  const pass = document.getElementById("password").value;
  const users = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {};

  if (!user || !pass) {
    document.getElementById("loginMessage").innerText = "Vui lòng nhập đầy đủ thông tin!";
    return;
  }

  if (users[user]) {
    document.getElementById("loginMessage").innerText = "Tài khoản đã tồn tại!";
  } else {
    users[user] = { password: pass };
    localStorage.setItem(STORAGE_KEY, JSON.stringify(users));
    localStorage.setItem(SESSION_KEY, JSON.stringify({ username: user }));
    localStorage.setItem('userData', JSON.stringify({ username: user }));
    document.getElementById("loginScreen").style.display = "none";
    document.getElementById("mainContent").style.display = "block";
    updateAccountInfoUI();
  }
}

// Tự động đăng nhập nếu đã lưu session
window.addEventListener("DOMContentLoaded", () => {
  const session = JSON.parse(localStorage.getItem(SESSION_KEY));
  if (session && session.username) {
    document.getElementById("loginScreen").style.display = "none";
    document.getElementById("mainContent").style.display = "block";
    updateAccountInfoUI();
  }
});

// Hàm cập nhật thông tin tài khoản trên UI (nếu có)
function updateAccountInfoUI() {
  const session = JSON.parse(localStorage.getItem(SESSION_KEY));
  if (session && session.username) {
    document.getElementById("accountName").innerText = session.username;
  }
}

window.addEventListener("DOMContentLoaded", () => {
  const session = JSON.parse(localStorage.getItem(SESSION_KEY));
  if (session && session.username) {
    document.getElementById("loginScreen").style.display = "none";
    switchTab("mainContent");
  }
});
</script>

<header id="header">Cloud Game Free <span id="clock"></span></header>

<!-- Giao diện đăng nhập -->

<div id="loginScreen" style="display: flex; align-items: center; justify-content: center; height: 100vh; flex-direction: column; color: white; background: url('https://i.pinimg.com/originals/3b/5d/77/3b5d77ece81fea5e42a3cb60a3a22e19.gif') no-repeat center center / cover;">
  <h2 style="margin-bottom:20px;">🔥 Cloud Game Đăng Nhập</h2>
  <input id="username" placeholder="Tên tài khoản" style="padding:10px;margin:5px;width:70%;max-width:300px;" />
  <input id="password" type="password" placeholder="Mật khẩu" style="padding:10px;margin:5px;width:70%;max-width:300px;" />
  <div style="margin-top:10px;">
    <button onclick="handleLogin()" style="padding:10px 20px;margin-right:10px;">Đăng Nhập</button>
    <button onclick="handleRegister()" style="padding:10px 20px;">Đăng Ký</button>
  </div>
  <p id="loginMessage" style="color:lime;margin-top:15px;"></p>
</div>


<!-- Giao diện chính -->
<div class="content" id="mainContent" style="display: block;">
  <button class="btn" onclick="showDeviceOptions()" id="addDeviceBtn">➕ Thêm Thiết Bị</button>
  <div id="deviceOptions" class="hidden">
    <p id="chooseDevice">Bạn muốn chọn thiết bị nào</p>
    <button class="btn" onclick="choosePro()">Thiết bị PRO</button>
    <button class="btn" onclick="hideAllOptions()">Đóng</button>
  </div>
  <div id="proOptions" class="hidden">
    <p>Vui lòng chọn thiết bị Pro</p>
    <button class="btn" onclick="addWalkingDead()">The Walking Dead(PRO)⬇️</button>
    <button class="btn" onclick="addCluserUnlimited()">Cluser Unlimited(PRO)🔥</button>
    <button class="btn" onclick="addNexusUnlimited()">Nexus Unlimited(PRO)🔥 </button>
    <button class="btn" onclick="addBatterUnlimited()">The Batter Bears Unlimited(PRO)🔥</button>
    <button class="btn" onclick="hideAllOptions()">Thoát</button>
  </div>
  <div id="deviceList" class="device-slider"></div>
</div>

<div id="vpnPanel" class="content">
    <h2>Thiết bị VPN</h2>
    <button class="btn" onclick="showVpnOptions()">➕ Thêm Thiết Bị VPN</button>
    <div id="vpnOptions" class="hidden">
      <p>Vui lòng chọn thiết bị VPN</p>
      <button class="btn" onclick="addVpnDevice1('App Launcher V3')">App Launcher V3</button>
      <button class="btn" onclick="addVpnDevice2('App Launcher V2')">App Launcher V2</button>
      <button class="btn" onclick="addVpnDevice3('Roblox')">Roblox</button>
      <button class="btn" onclick="addVpnDevice4('Blox Fruit')">Blox Fruit</button>
      <button class="btn" onclick="addVpnDevice5('Genshin Impact')">Genshin Impact</button>
      <button class="btn" onclick="addVpnDevice6('Discord')">Discord</button>
      <button class="btn" onclick="addVpnDevice7('Stumble Guys')">Stumble Guys</button>
      <button class="btn" onclick="addVpnDevice8('Indian Bikes Driving 3D')">Indian Bikes Driving 3D</button>
      <button class="btn" onclick="addVpnDevice9('Call Of Duty ')">Call Of Duty </button>
      <button class="btn" onclick="addVpnDevice10('Final Fantasy')">Final Fantasy</button>
      <button class="btn" onclick="addVpnDevice11('Free Fire')">Free Fire</button>
      <button class="btn" onclick="addVpnDevice12('Pubg Mobile')">Pubg Mobile</button>
      <button class="btn" onclick="addVpnDevice13('Fortnite Mobile)">Fortnite Mobile</button>
      <button class="btn" onclick="addVpnDevice14('Among Us')">Among Us</button>
      <button class="btn" onclick="addVpnDevice15('Geometry Dash')">Geometry Dash</button>
      <button class="btn" onclick="addVpnDevice16('Honkai Star Rail')">Honkai Star Rail</button>
      <button class="btn" onclick="addVpnDevice17('Shadow Light 3(No Vpn)')">Shadow Light 3 (No Vpn)</button>
      <button class="btn" onclick="addVpnDevice18('Aptoide')">Aptoide</button>
      <button class="btn" onclick="addVpnDevice19('App Launcher V1')">App Launcher V1</button>
      <button class="btn".="hideVpnOptions()">Đóng</button>
    </div>
    <div id="vpnDeviceList" class="device-slider"></div>
  </div>
<!-- Cài đặt -->
<div id="settingPanel" class="content">
  <h2>Cài đặt</h2>
  <label>Chế độ giao diện:
    <select id="themeMode" onchange="toggleTheme(this.value)">
      <option value="light">Sáng</option>
      <option value="dark">Tối</option>
    </select>
    </label>
  <label>Hình nền mặc định:
    <div class="bg-container">
        <div class="bg-sample" onclick="setBackground('pink')" style="background:pink"></div>
        <div class="bg-sample" onclick="setBackground('linear-gradient(to right, #00f260, #0575e6)')" style="background:linear-gradient(to right, #00f260, #0575e6)"></div>
        <div class="bg-sample" onclick="setBackground('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTqayx7w6bhFqAJ4RHfuYQepW2LftLzqeN16w&s')" style="background-image:url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTqayx7w6bhFqAJ4RHfuYQepW2LftLzqeN16w&s')"></div>
        <div class="bg-sample" onclick="setBackground('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTXgmSZzH48WjQ8i3v-9Rr06HECbh10gXZEwQ&usqp=CAU')" style="background-image:url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTXgmSZzH48WjQ8i3v-9Rr06HECbh10gXZEwQ&usqp=CAU')"></div>
        <div class="bg-sample" onclick="setBackground('https://www.gifcen.com/wp-content/uploads/2022/10/zenitsu-gif-8.gif')" style="background-image:url('https://www.gifcen.com/wp-content/uploads/2022/10/zenitsu-gif-8.gif')"></div>
        <div class="bg-sample" onclick="setBackground('https://i.pinimg.com/originals/0d/ba/9f/0dba9fd5b9bbfe556fe694051584df1c.gif')" style="background-image:url('https://i.pinimg.com/originals/0d/ba/9f/0dba9fd5b9bbfe556fe694051584df1c.gif')"></div>
        <div class="bg-sample" onclick="setBackground('https://gifdb.com/images/high/luffy-gear-5-498-x-280-gif-584tc82d0zydwcqw.webp')" style="background-image:url('https://gifdb.com/images/high/luffy-gear-5-498-x-280-gif-584tc82d0zydwcqw.webp')"></div>
        <div class="bg-sample" onclick="setBackground('https://gifdb.com/images/high/luffy-gear-5-498-x-498-gif-eavvc1mtliqbbz9i.webp')" style="background-image:url('https://gifdb.com/images/high/luffy-gear-5-498-x-498-gif-eavvc1mtliqbbz9i.webp')"></div>
        <div class="bg-sample" onclick="setBackground('https://giffiles.alphacoders.com/221/221275.gif')" style="background-image:url('https://giffiles.alphacoders.com/221/221275.gif')"></div>
        <div class="bg-sample" onclick="setBackground('https://www.icegif.com/wp-content/uploads/2022/04/icegif-1195.gif')" style="background-image:url('https://www.icegif.com/wp-content/uploads/2022/04/icegif-1195.gif')"></div>
        <div class="bg-sample" onclick="setBackground('https://www.icegif.com/wp-content/uploads/2023/10/icegif-875.gif')" style="background-image:url('https://www.icegif.com/wp-content/uploads/2023/10/icegif-875.gif')"></div>
        <div class="bg-sample" onclick="setBackground('https://i.pinimg.com/originals/3b/5d/77/3b5d77ece81fea5e42a3cb60a3a22e19.gif')" style="background-image:url('https://i.pinimg.com/originals/3b/5d/77/3b5d77ece81fea5e42a3cb60a3a22e19.gif')"></div>
      </div>
  </label>
  <button class="btn" onclick="switchTab('mainContent')">🔙 Quay lại</button>
</div>

<!-- Trợ giúp -->
<div id="helpPanel" class="content">
  <h2 style="margin-left:10px;">🎥 Video Hướng Dẫn</h2>
  <div style="display: flex; gap: 10px; overflow-x: auto; padding: 10px;">
    
    <div style="flex: 0 0 300px; background: #fff; border-radius: 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); padding: 10px;">
      <h4 style="margin: 5px 0;">Hướng dẫn sử dụng</h4>
      <video width="100%" controls style="border-radius: 12px;">
        <source src="huongdan1.mp4" type="video/mp4">
        Trình duyệt không hỗ trợ video.
      </video>
    </div>

    <!-- Bạn có thể thêm nhiều video tương tự -->
    <div style="flex: 0 0 300px; background: #fff; border-radius: 16px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); padding: 10px;">
      <h4 style="margin: 5px 0;">Hướng dẫn nâng cao</h4>
      <video width="100%" controls style="border-radius: 12px;">
        <source src="huongdan2.mp4" type="video/mp4">
        Trình duyệt không hỗ trợ video.
      </video>
    </div>

  </div>
</div>


<!-- Tài khoản -->
<div id="accountPanel" class="content">
  <h2>Thông tin tài khoản</h2>
  <p><strong>Tên tài khoản:</strong> <span id="accountName">Chưa đăng nhập</span></p>
  <button onclick="changeAccountName()">Đổi tên tài khoản</button>
  <button onclick="deleteAccount()">Xoá tài khoản</button>
</div>

<!-- Các nút chuyển tab -->
<div class="tabbar">
  <button class="btn" onclick="switchTab('mainContent')">📱 Thiết Bị PRO</button>
  <button class="btn" onclick="switchTab('vpnPanel')">📱Thiết Bị VPN</button>
  <button class="btn" onclick="switchTab('settingPanel')">⚙️ Cài đặt</button>
  <button class="btn" onclick="switchTab('helpPanel')">❓ Trợ giúp</button>
  <button class="btn" onclick="switchTab('accountPanel')">👤Tài khoản</button>
</div>


<div id="iphoneView">
  <div class="iphone-shell">
    <div class="iphone-notch"></div>
    <iframe id="webFrame" class="iphone-frame"></iframe>
    
    <div id="iframeControls" style="position: absolute; bottom: 20px; left: 50%; transform: translateX(-50%); z-index: 9999; display: flex; gap: 10px;">
      <button onclick="backToMain()" style="padding: 6px 14px; border-radius: 6px; border: none; background: #ffffffcc; font-weight: bold;">Quay lại</button>
      <button onclick="reloadIframe()" style="padding: 6px 14px; border-radius: 6px; border: none; background: #ffffffcc; font-weight: bold;">Tải lại</button>
    </div>

<script>
  function updateClock() {
    const now = new Date();
    document.getElementById('clock').innerText = now.toLocaleTimeString();
  }
  setInterval(updateClock, 1000);
  updateClock();

  function switchTab(tabId) {
    document.querySelectorAll('.content').forEach(c => c.style.display = 'none');
    document.getElementById(tabId).style.display = 'block';
  }

  function toggleTheme(mode) {
    if (mode === 'dark') document.body.classList.add('dark');
    else document.body.classList.remove('dark');
  }

  function setBackground(value) {
    document.body.style.background = value;
  }

  function showDeviceOptions() {
    hideAllOptions();
    document.getElementById('deviceOptions').classList.remove('hidden');
  }

  function choosePro() {
    hideAllOptions();
    document.getElementById('proOptions').classList.remove('hidden');
  }

  function hideAllOptions() {
    document.getElementById('deviceOptions').classList.add('hidden');
    document.getElementById('proOptions').classList.add('hidden');
  }

  function addDevice(name, url, containerId = 'deviceList', isVpn = false) {
    const id = name.replace(/\s+/g, '-') + '-' + Date.now();
    const now = new Date().toLocaleString();
    const div = document.createElement('div');
    div.className = 'device-card';
    div.id = id;

    div.innerHTML = `
      <div class="device-title">${name}</div>
      <div class="device-time">Tạo lúc: ${now}</div>
      <iframe src="${url}" style="width: 100%; height: 300px; border: none; border-radius: 16px; background: #111;"></iframe>
      <div class="device-actions">
        <button class="btn" onclick="${isVpn ? `window.open('${url}', '_blank')` : `startDevice('${url}')`}">Khởi động</button>
        <button class="btn" onclick="removeDevice('${id}')">Xoá</button>
      </div>
    `;

    document.getElementById(containerId).appendChild(div);
    hideAllOptions();
  }

  function removeDevice(id) {
    const el = document.getElementById(id);
    if (el) el.remove();
  }

  function startDevice(url) {
    document.getElementById('iphoneView').style.display = 'block';
    document.getElementById('webFrame').src = url;
  }

  function backToMain() {
    document.getElementById('iphoneView').style.display = 'none';
  }

function addWalkingDead() {
      addDevice("The Walking Dead(PRO)⬇️", "https://turbolite.vercel.app/server/url?data=aHR0cHM6Ly91cHJveHkub25saW5lL3VsdHJhdmlvbGV0L2h2dHJzOCUyRi1obXN2ZWYtMS5sb3UuZWctYXJwcSUyRmVhbmF6eSUyRnBuYSU3Qi12ZWFobG9ub2V5JTJGbGtta3RnZC0xMjUzMi10amUlMkZ3Y2xpaWxnJTJGZGdhZi1xdXB2a3ZtcnEuanRvbA==&fs=0");
    }

    function addCluserUnlimited() {
      addDevice("Cluser Unlimited(PRO)🔥", "https://turbolite.vercel.app/server/url?data=aHR0cHM6Ly91cHJveHkub25saW5lL3VsdHJhdmlvbGV0L2h2dHJzOCUyRi1ubXclMkNnZSUyRmNwcnMtY251cXRnciUyRmlsYy05MzU1JTJGYWx3c3ZlcC5qdG9s&fs=0");
    }

    function addNexusUnlimited() {
      addDevice("Nexus Unlimited(PRO)🔥 ", "https://turbolite.vercel.app/server/url?data=aHR0cHM6Ly91cHJveHkub25saW5lL3VsdHJhdmlvbGV0L2h2dHJzOCUyRi10Z3N2ZHBpdGU2LmxvdS5lZy1hcnBxJTJGb2FlaWEtZWFvZS04NTA0JTJGbGV6dXEtbGUlNjB1bmElMkZlYWhtZXEuanRvbA==&fs=0");
    }

    function addBatterUnlimited() {
      addDevice("The Batter Bears Unlimited(PRO)🔥", "https://turbolite.vercel.app/server/url?data=aHR0cHM6Ly91cHJveHkub25saW5lL3VsdHJhdmlvbGV0L2h2dHJzOCUyRi10Z3N2ZHBpdGU2LmxvdS5lZy1hcnBxJTJGJTYwYXZ0bmVhb2tuJTJGZ2NtZ3MtODA3Ml92MS1iY3R2bGctJTYwZWNycS1qZXBvZ3MlMkNodm1u&fs=0");
    }

    // VPN functions
    function showVpnOptions() {
      document.getElementById('vpnOptions').classList.remove('hidden');
    }

    function hideVpnOptions() {
      document.getElementById('vpnOptions').classList.add('hidden');
    }

    function addVpnDevice1(VPN) {
      addDevice(VPN, "https://testdrive-int2.now.gg/apps/uncube/7074/now.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice2(VPN) {
      addDevice(VPN, "https://now.gg/apps/uncube/10005/now.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice3(VPN) {
      addDevice(VPN, "https://mathstutor.life", "vpnDeviceList", true);
    }
    
    function addVpnDevice4(VPN) {
      addDevice(VPN, "https://now.gg/apps/Blox-fruit/19901/Blox-fruit.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice5(VPN) {
      addDevice(VPN, "https://now.gg/apps/cognosphere-pte-ltd-/1773/genshin-impact.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice6(VPN) {
      addDevice(VPN, "https://now.gg/apps/discord-inc-/1430/discord-talk-chat-hang-out.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice7(VPN) {
      addDevice(VPN, "https://now.gg/apps/a/10011/b.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice8(VPN) {
      addDevice(VPN, "https://now.gg/apps/rohit-gaming-studio/8822/indian-bikes-driving-3d.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice9(VPN) {
      addDevice(VPN, "https://now.gg/apps/a/10008/b.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice10(VPN) {
      addDevice(VPN, "https://now.gg/apps/a/10018/b.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice11(VPN) {
      addDevice(VPN, "https://now.gg/apps/garena-international-i/1398/free-fire.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice12(VPN) {
      addDevice(VPN, "https://now.gg/apps/proxima-beta/2609/pubg-mobile-resistance.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice13(VPN) {
      addDevice(VPN, "https://now.gg/apps/a/10004/b.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice14(VPN) {
      addDevice(VPN, "https://now.gg/apps/innersloth-llc/4047/among-us.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice15(VPN) {
      addDevice(VPN, "https://now.gg/apps/griffpatch/51387/geometry-dash.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice16(VPN) {
      addDevice(VPN, "https://now.gg/apps/cognosphere-pte-ltd/1591/honkai-star-rail.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice17(VPN) {
      addDevice(VPN, "https://webproxy.proxyshare.com/sencure/TGO03g9eGmAm8VljKoHO6", "vpnDeviceList", true);
    }
 
    function addVpnDevice18(VPN) {
      addDevice(VPN, "https://now.gg/apps/aptoide/5874/aptoide.html", "vpnDeviceList", true);
    }
    
    function addVpnDevice19(VPN) {
      addDevice(VPN, "https://now.gg/apps/uncube/7074/now.html", "vpnDeviceList", true);
    }
 
 
  // Tài khoản
  function updateAccountInfoUI() {
    const userData = JSON.parse(localStorage.getItem('userData') || '{}');
    const name = userData.username || 'Chưa đăng nhập';
    document.getElementById('accountName').innerText = name;
  }

  function changeAccountName() {
    const newName = prompt("Nhập tên tài khoản mới:");
    if (newName && newName.trim() !== "") {
      const userData = JSON.parse(localStorage.getItem('userData') || '{}');
      userData.username = newName.trim();
      localStorage.setItem('userData', JSON.stringify(userData));
      updateAccountInfoUI();
      alert("Đã đổi tên tài khoản thành công!");
    }
  }

  function deleteAccount() {
  if (confirm("Bạn có chắc chắn muốn xoá tài khoản?")) {
    localStorage.removeItem('userData');
    localStorage.removeItem('devices');
    localStorage.removeItem('vmSettings');
    localStorage.removeItem('cloudgame_session'); // ✅ Xoá session để không tự động đăng nhập
    localStorage.removeItem('cloudgame_accounts'); // (Tùy chọn) Nếu muốn xoá luôn tài khoản khỏi hệ thống

    alert("Đã xoá tài khoản!");
    location.reload();
  }
}

function setBackground(url) {
  document.body.style.backgroundImage = `url('${url}')`;
  document.body.style.backgroundSize = 'cover';
  document.body.style.backgroundRepeat = 'no-repeat';
  document.body.style.backgroundPosition = 'center';
  localStorage.setItem('backgroundImage', url); // ✅ Lưu vào localStorage
}

  // Gọi tự động khi mở trang
  window.addEventListener('load', updateAccountInfoUI);
  
  window.addEventListener('load', () => {
  const savedBg = localStorage.getItem('backgroundImage');
  if (savedBg) {
    document.body.style.backgroundImage = `url('${savedBg}')`;
    document.body.style.backgroundSize = 'cover';
    document.body.style.backgroundRepeat = 'no-repeat';
    document.body.style.backgroundPosition = 'center';
  }
});

 </script> 
  function backToMain() {
    document.getElementById("iphoneView").style.display = "none";
    document.getElementById("mainScreen")?.style.display = "block"; // Đảm bảo bạn có phần tử này
  }

  function reloadIframe() {
    const iframe = document.getElementById("webFrame");
    if (iframe) iframe.contentWindow.location.reload();
  }

</script>
</body>
</html>
