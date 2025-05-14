<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Free Fire Diamond Top-up</title>
  <style>
    body { font-family: sans-serif; background: #f9f9f9; padding: 20px; }
    .container { max-width: 600px; margin: auto; background: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
    h1 { text-align: center; color: #e8491d; }
    label { display: block; margin: 10px 0 5px; }
    select, input[type="text"], input[type="number"] {
      width: 100%; padding: 10px; border: 1px solid #ccc; border-radius: 4px;
    }
    button { background: #e8491d; color: #fff; border: none; padding: 12px; width: 100%; border-radius: 5px; cursor: pointer; margin-top: 15px; }
    button:hover { background: #cf3c14; }
    .success { text-align: center; color: green; margin-top: 20px; font-weight: bold; }
    .whatsapp { text-align: center; margin-top: 20px; }
    .whatsapp a { text-decoration: none; color: #25D366; font-weight: bold; }
    .notice { margin-top: 20px; text-align: center; font-size: 15px; color: #e8491d; font-weight: bold; }
  </style>
</head>
<body>
  <div class="container">
    <h1>Free Fire Diamond Top-up</h1>

    <form id="orderForm">
      <label for="product">Select Product</label>
      <select id="product" required>
        <option value="">--Choose--</option>
        <option value="115">115 Diamonds - 85৳</option>
        <option value="240">240 Diamonds - 160৳</option>
        <option value="480">480 Diamonds - 240৳</option>
        <option value="610">610 Diamonds - 400৳</option>
        <option value="1240">1240 Diamonds - 800৳</option>
        <option value="2530">2530 Diamonds - 1600৳</option>
        <option value="weekly">Weekly Membership - 165৳</option>
        <option value="monthly">Monthly Membership - 800৳</option>
      </select>

      <label for="uid">Free Fire UID</label>
      <input type="text" id="uid" required placeholder="Enter your Free Fire UID">

      <label for="name">In-Game Name</label>
      <input type="text" id="name" required placeholder="Enter your in-game name">

      <label for="trxid">bKash Transaction ID</label>
      <input type="text" id="trxid" required placeholder="Enter TrxID after payment">

      <button type="submit">Submit Order</button>
    </form>

    <div class="success" id="successMsg" style="display:none;">
      আপনার অর্ডার প্রসেসিং হচ্ছে...
    </div>

    <div class="whatsapp">
      যোগাযোগ: <a href="https://wa.me/8801947612356" target="_blank">WhatsApp Chat</a>
    </div>

    <p style="text-align:center; margin-top:10px;">
      Payment to: <strong>bKash Personal - 01741986806 (Send Money)</strong>
    </p>

    <p class="notice">
      ১০ মিনিটের ভিতরে ডেলিভারি না পেলে হোয়াটসঅ্যাপে স্ক্রিনশট এবং গেমের ইউআইডি দিয়ে রাখবেন।
    </p>
  </div>

  <script>
    document.getElementById('orderForm').addEventListener('submit', function(e) {
      e.preventDefault();
      document.getElementById('successMsg').style.display = 'block';
    });
  </script>
</body>
</html>
