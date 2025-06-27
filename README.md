<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>X-VPN Login</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #2f3542, #1e90ff);
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
    }

    .login-box {
      background: #fff;
      padding: 40px;
      border-radius: 15px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.3);
      width: 90%;
      max-width: 350px;
      text-align: center;
      margin-top: 30px;
    }

    .login-box h2 {
      margin-bottom: 25px;
      color: #1e90ff;
    }

    .login-box input[type="text"],
    .login-box input[type="password"] {
      width: 100%;
      padding: 12px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
    }

    .login-box input[type="checkbox"] {
      margin-right: 5px;
    }

    .login-box button {
      background: #1e90ff;
      color: white;
      border: none;
      padding: 12px 25px;
      margin-top: 15px;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
      transition: 0.3s;
    }

    .login-box button:hover {
      background: #3742fa;
    }

    .message {
      margin-top: 15px;
      color: red;
      font-size: 14px;
    }

    .server-status {
      margin-top: 40px;
      background: #fff;
      padding: 20px;
      border-radius: 15px;
      width: 90%;
      max-width: 400px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.2);
    }

    .server-status h3 {
      color: #1e90ff;
      margin-bottom: 15px;
    }

    .server {
      display: flex;
      justify-content: space-between;
      padding: 10px 0;
      border-bottom: 1px solid #ddd;
      font-size: 16px;
    }

    .server:last-child {
      border-bottom: none;
    }

    .online {
      color: green;
      font-weight: bold;
    }

    .offline {
      color: red;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <div class="login-box">
    <h2>X-VPN Login</h2>
    <input type="text" id="username" placeholder="Username" />
    <input type="password" id="password" placeholder="Password" />
    <div style="text-align: left; margin-top: 10px;">
      <input type="checkbox" onclick="togglePassword()"> Show Password
    </div>
    <button onclick="login()">Login</button>
    <div class="message" id="msgBox"></div>
  </div>

  <div class="server-status">
    <h3>🔌 VPN Server Status</h3>
    <div class="server">
      <span>🇺🇸 USA - 185.123.45.67:1194</span>
      <span id="srv1" class="online">Online</span>
    </div>
    <div class="server">
      <span>🇸🇬 Singapore - 91.200.34.21:443</span>
      <span id="srv2" class="offline">Offline</span>
    </div>
    <div class="server">
      <span>🇩🇪 Germany - 80.55.22.33:2080</span>
      <span id="srv3" class="online">Online</span>
    </div>
  </div>

  <script>
    function togglePassword() {
      const pass = document.getElementById("password");
      pass.type = pass.type === "password" ? "text" : "password";
    }

    function login() {
      const user = document.getElementById("username").value;
      const pass = document.getElementById("password").value;
      const msg = document.getElementById("msgBox");

      if (user === "admin" && pass === "123456") {
        msg.style.color = "green";
        msg.innerText = "Login successful! Redirecting...";
        setTimeout(() => {
          window.location.href = "dashboard.html"; // আপনার পেজের লিংক
        }, 1500);
      } else {
        msg.style.color = "red";
        msg.innerText = "Invalid username or password.";
      }
    }

    // Future enhancement: বাস্তব স্ট্যাটাস চেক করতে backend API লাগবে
    // fetch("/api/check-server-status").then(...)
  </script>

</body>
</html>
