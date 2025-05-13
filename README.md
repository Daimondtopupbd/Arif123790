<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Diamond Topup BD</title>
  
  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.18.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.18.0/firebase-auth.js"></script>

  <!-- Firebase Configuration -->
  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_AUTH_DOMAIN",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_STORAGE_BUCKET",
      messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
      appId: "YOUR_APP_ID",
      measurementId: "YOUR_MEASUREMENT_ID"
    };

    const app = firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
  </script>

</head>
<body>
  <h1>Welcome to Diamond Topup BD</h1>
  
  <!-- Google Login Button -->
  <button id="google-login-btn">Login with Google</button>

  <!-- Phone Login Section -->
  <input type="text" id="phone-number" placeholder="Enter Phone Number">
  <button id="phone-login-btn">Login with Phone</button>

  <div id="verification-code-container" style="display:none;">
    <input type="text" id="verification-code" placeholder="Enter Verification Code">
    <button id="verify-code-btn">Verify Code</button>
  </div>

  <!-- Order Form -->
  <section class="order-form" style="padding: 20px; background: #fff;">
    <h2 style="text-align:center;">Place Your Order</h2>
    <form action="submit_order.php" method="POST" style="max-width: 400px; margin: auto;">
      <label>Free Fire UID:</label><br>
      <input type="text" name="uid" required style="width: 100%; padding: 10px; margin: 10px 0;"><br>

      <label>Select Diamond Package:</label><br>
      <select name="package" required style="width: 100%; padding: 10px; margin: 10px 0;">
        <option value="">--Select--</option>
        <option value="100">100 Diamond - ৳90</option>
        <option value="210">210 Diamond - ৳180</option>
        <option value="530">530 Diamond - ৳450</option>
        <option value="1060">1060 Diamond - ৳850</option>
      </select><br>

      <label>Payment Method:</label><br>
      <select name="payment_method" required style="width: 100%; padding: 10px; margin: 10px 0;">
        <option value="">--Select--</option>
        <option value="Bkash">Bkash</option>
        <option value="Nagad">Nagad</option>
        <option value="Rocket">Rocket</option>
      </select><br>

      <label>Transaction ID:</label><br>
      <input type="text" name="trx_id" required style="width: 100%; padding: 10px; margin: 10px 0;"><br>

      <button type="submit" style="width: 100%; padding: 12px; background: #28a745; color: white; border: none; border-radius: 5px;">Submit Order</button>
    </form>
  </section>

  <script>
    // Google Login Functionality
    const googleLoginButton = document.getElementById('google-login-btn');
    googleLoginButton.addEventListener('click', () => {
      const provider = new firebase.auth.GoogleAuthProvider();
      auth.signInWithPopup(provider)
        .then((result) => {
          const user = result.user;
          console.log("Google Login Success: ", user);
          // Redirect or store user data
        })
        .catch((error) => {
          console.error("Google Login Error: ", error.message);
        });
    });

    // Phone Login Functionality
    const phoneLoginButton = document.getElementById('phone-login-btn');
    const phoneNumberInput = document.getElementById('phone-number');
    const verificationCodeContainer = document.getElementById('verification-code-container');
    const verificationCodeInput = document.getElementById('verification-code');
    const verifyCodeButton = document.getElementById('verify-code-btn');
    
    let verificationId;

    phoneLoginButton.addEventListener('click', () => {
      const phoneNumber = phoneNumberInput.value;
      const appVerifier = window.recaptchaVerifier; // Make sure reCAPTCHA is initialized

      auth.signInWithPhoneNumber(phoneNumber, appVerifier)
        .then((confirmationResult) => {
          verificationId = confirmationResult.verificationId;
          verificationCodeContainer.style.display = 'block';
        })
        .catch((error) => {
          console.error("Phone Login Error: ", error.message);
        });
    });

    verifyCodeButton.addEventListener('click', () => {
      const code = verificationCodeInput.value;
      const credential = firebase.auth.PhoneAuthProvider.credential(verificationId, code);

      auth.signInWithCredential(credential)
        .then((result) => {
          const user = result.user;
          console.log("Phone Login Success: ", user);
          // Redirect or store user data
        })
        .catch((error) => {
          console.error("Phone Verification Error: ", error.message);
        });
    });
  </script>

</body>
</html>

submit_order.php – অর্ডার সাবমিট সিস্টেম

<?php
// Database connection
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "topupdb";

$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

// Get form data
$uid = $_POST['uid'];
$package = $_POST['package'];
$payment_method = $_POST['payment_method'];
$trx_id = $_POST['trx_id'];

// Insert order
$sql = "INSERT INTO orders (uid, package, payment_method, trx_id)
        VALUES ('$uid', '$package', '$payment_method', '$trx_id')";

if ($conn->query($sql) === TRUE) {
  $order_id = $conn->insert_id; // Get last inserted ID
  // Redirect to confirmation page with order ID
  header("Location: order_success.php?id=$order_id");
  exit();
} else {
  echo "Error: " . $conn->error;
}

$conn->close();
?>

order_success.php – অর্ডার কনফার্মেশন পেজ

<?php
$order_id = isset($_GET['id']) ? $_GET['id'] : 0;
?>

<!DOCTYPE html>
<html>
<head>
  <title>Order Successful</title>
</head>
<body>
  <h2>Thank You! Your Order Has Been Placed.</h2>
  <p>Your Order ID: <strong>#<?= htmlspecialchars($order_id) ?></strong></p>
  <p>We will deliver the diamonds shortly after verifying the payment.</p>
  <p>If urgent, contact us on <a href="https://wa.me/8801947612356" target="_blank">WhatsApp</a></p>
  <br>
  <a href="index.html">Go Back to Home</a>
</body>
</html>


---

4. Admin Panel (PHP)

admin_login.php – অ্যাডমিন লগইন ফর্ম

<?php
session_start();
if (isset($_SESSION['admin_logged_in'])) {
  header("Location: admin_dashboard.php");
  exit();
}
?>

<!DOCTYPE html>
<html>
<head>
  <title>Admin Login</title>
</head>
<body>
  <h2>Admin Login</h2>
  <form method="POST" action="admin_login.php">
    <input type="text" name="username" placeholder="Username" required><br><br>
    <input type="password" name="password" placeholder="Password" required><br><br>
    <button type="submit">Login</button>
  </form>

  <?php
  if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $username = $_POST['username'];
    $password = $_POST['password'];

    // Replace with your desired username/password
    if ($username === 'admin' && $password === '1234') {
      $_SESSION['admin_logged_in'] = true;
      header("Location: admin_dashboard.php");
      exit();
    } else {
      echo "<p style='color:red;'>Invalid credentials!</p>";
    }
  }
  ?>
</body>
</html>

admin_dashboard.php – অ্যাডমিন ড্যাশবোর্ড

<?php
session_start();
if (!isset($_SESSION['admin_logged_in'])) {
  header("Location: admin_login.php");
  exit();
}

$conn = new mysqli("localhost", "root", "", "topupdb");

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}

$sql = "SELECT * FROM orders ORDER BY order_time DESC";
$result = $conn->query($sql);
?>

<!DOCTYPE html>
<html>
<head>
  <title>Admin Dashboard</title>
  <style>
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
  </style>
</head>
<body>
  <h2>Admin Dashboard</h2>
  <p><a href="admin_logout.php">Logout</a></p>

  <table>
    <tr>
      <th>ID</th>
      <th>UID</th>
      <th>Package</th>
      <th>Payment Method</th>
      <th>TrxID</th>
      <th>Time</th>
    </tr>
    <?php while($row = $result->fetch_assoc()) { ?>
    <tr>
      <td><?= $row['id'] ?></td>
      <td><?= $row['uid'] ?></td>
      <td><?= $row['package'] ?></td>
      <td><?= $row['payment_method'] ?></td>
      <td><?= $row['trx_id'] ?></td>
      <td><?= $row['order_time'] ?></td>
    </tr>
    <?php } ?>
  </table>
</body>
</html>

admin_logout.php – লগআউট স্ক্রিপ্ট

<?php
session_start();
session_destroy();
header("Location: admin_login.php");
exit();
