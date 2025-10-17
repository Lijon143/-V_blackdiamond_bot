<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Earning App - Web Version</title>
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <style>
    :root {
      --primary-color: #007bff;
      --secondary-color: #28a745;
      --background-color: #f9fafc;
      --card-background: #ffffff;
      --text-color: #333;
      --border-color: #e0e0e0;
    }

    body {
      margin: 0;
      font-family: "Segoe UI", sans-serif;
      background-color: var(--background-color);
      color: var(--text-color);
    }

    header {
      background-color: var(--card-background);
      padding: 15px 30px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid var(--border-color);
      position: sticky;
      top: 0;
      z-index: 100;
    }

    header .brand {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    header img {
      width: 45px;
      height: 45px;
      border-radius: 50%;
      border: 2px solid var(--primary-color);
      object-fit: cover;
    }

    nav {
      display: flex;
      gap: 20px;
    }

    nav button {
      background: none;
      border: none;
      font-size: 15px;
      font-weight: 600;
      color: var(--text-color);
      cursor: pointer;
      padding: 10px;
      border-radius: 8px;
      transition: background-color 0.3s;
    }

    nav button:hover, nav button.active {
      background-color: var(--primary-color);
      color: #fff;
    }

    main {
      padding: 30px 40px;
      max-width: 1200px;
      margin: 0 auto;
    }

    .page {
      display: none;
      animation: fadeIn 0.4s ease-in-out;
    }

    .page.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .card {
      background-color: var(--card-background);
      border-radius: 12px;
      padding: 25px;
      box-shadow: 0 3px 10px rgba(0,0,0,0.05);
      border: 1px solid var(--border-color);
      margin-bottom: 25px;
    }

    .card h2 {
      display: flex;
      align-items: center;
      gap: 10px;
      font-size: 20px;
      margin-top: 0;
    }

    .balance {
      font-size: 40px;
      font-weight: bold;
      color: var(--secondary-color);
      margin: 10px 0;
    }

    .dashboard-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
    }

    .dashboard-item {
      background: var(--card-background);
      border: 1px solid var(--border-color);
      border-radius: 12px;
      text-align: center;
      padding: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
    }

    .dashboard-item .material-icons {
      font-size: 35px;
      color: var(--primary-color);
      margin-bottom: 10px;
    }

    .primary-btn, .secondary-btn {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      border: none;
      padding: 15px 25px;
      border-radius: 10px;
      font-size: 16px;
      font-weight: 600;
      cursor: pointer;
      transition: transform 0.2s;
      margin-right: 10px;
    }

    .primary-btn {
      background-color: var(--primary-color);
      color: white;
    }

    .primary-btn:hover { transform: translateY(-2px); }

    .secondary-btn {
      background-color: #f2f2f2;
      border: 1px solid var(--border-color);
      color: var(--text-color);
    }

    footer {
      text-align: center;
      padding: 20px;
      background-color: var(--card-background);
      border-top: 1px solid var(--border-color);
      margin-top: 40px;
      color: var(--text-color);
    }

    @media (max-width: 768px) {
      main { padding: 20px; }
      nav { flex-wrap: wrap; justify-content: center; }
    }
  </style>
</head>
<body>
  <header>
    <div class="brand">
      <img id="header-profile-pic" src="https://via.placeholder.com/45" alt="Profile">
      <h2 id="header-user-name">User</h2>
    </div>
    <nav>
      <button class="active" data-page="home">Home</button>
      <button data-page="earn">Earn</button>
      <button data-page="withdraw">Withdraw</button>
      <button data-page="profile">Profile</button>
    </nav>
  </header>

  <main>
    <!-- Home -->
    <section id="home" class="page active">
      <div class="card">
        <h2><span class="material-icons">account_balance_wallet</span>My Balance</h2>
        <div class="balance" id="current-balance">BDT 0.00</div>
        <button class="primary-btn" id="start-earning-btn">Start Earning<span class="material-icons">arrow_forward</span></button>
        <button class="secondary-btn" id="withdraw-btn">Withdraw<span class="material-icons">currency_exchange</span></button>
      </div>
      <div class="dashboard-grid">
        <div class="dashboard-item">
          <span class="material-icons">attach_money</span>
          <h3 id="total-earnings">BDT 0.00</h3>
          <p>Total Earnings</p>
        </div>
        <div class="dashboard-item">
          <span class="material-icons">visibility</span>
          <h3 id="ads-watched">0</h3>
          <p>Ads Watched</p>
        </div>
        <div class="dashboard-item">
          <span class="material-icons">group</span>
          <h3 id="total-referrals">0</h3>
          <p>Total Referrals</p>
        </div>
      </div>
    </section>

    <!-- Earn -->
    <section id="earn" class="page">
      <div class="card">
        <h2><span class="material-icons">play_circle</span>Earn Rewards</h2>
        <p id="ad-progress-text">Completed: 0 / 50</p>
        <div style="height:10px;background:#eee;border-radius:5px;overflow:hidden;">
          <div id="ad-progress-bar" style="height:100%;width:0;background:var(--primary-color);transition:0.5s;"></div>
        </div>
        <button class="primary-btn" id="watch-ad-btn">Watch Ad <span class="material-icons">play_circle_filled</span></button>
      </div>
    </section>

    <!-- Withdraw -->
    <section id="withdraw" class="page">
      <div class="card">
        <h2><span class="material-icons">currency_exchange</span>Withdraw</h2>
        <div class="balance" id="withdraw-balance-display">BDT 0.00</div>
        <p>Minimum withdraw amount: BDT 100</p>
        <form id="withdraw-form" style="margin-top:20px;">
          <label>Method:</label>
          <select id="withdraw-method" required>
            <option value="" disabled selected>Select a method</option>
            <option value="bkash">bKash</option>
            <option value="binance">Binance</option>
          </select>
          <br><br>
          <label>Account Number:</label>
          <input type="text" id="account-number" required>
          <br><br>
          <label>Amount (BDT):</label>
          <input type="number" id="withdraw-amount" min="100" required>
          <br><br>
          <button type="submit" class="primary-btn">Submit Withdraw</button>
        </form>
      </div>
    </section>

    <!-- Profile -->
    <section id="profile" class="page">
      <div class="card">
        <h2><span class="material-icons">person</span>My Profile</h2>
        <p><strong>Name:</strong> <span id="profile-user-name">User</span></p>
        <p><strong>Username:</strong> <span id="profile-user-username">@telegram</span></p>
        <p><strong>Total Earnings:</strong> <span id="profile-earnings">BDT 0.00</span></p>
        <p><strong>Ads Watched:</strong> <span id="profile-ads-watched">0</span></p>
      </div>
    </section>
  </main>

  <footer>
    © 2025 My Earning App | Powered by V.ʙʟᴀᴄᴋ ᴅɪᴀᴍᴏɴᴅ.bot
  </footer>

  <script src='//libtl.com/sdk.js' data-zone='10043368' data-sdk='show_10043368'></script>

  <script>
    const tg = window
