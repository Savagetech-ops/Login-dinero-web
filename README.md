# Login-dinero-web
<!-- Project: Dinero Web Platform - Phase 5: WhatsApp Auto Alert -->
<!-- Includes login, dashboard, Firebase Auth, Firestore, and WhatsApp notification -->

<!-- login.html and dashboard.html from previous phase remain unchanged except the dashboard script update below -->

<!-- 2. dashboard.html (updated with WhatsApp alert) -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>User Dashboard</title>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js"></script>
  <style>
    body {
      font-family: Arial;
      background: linear-gradient(to right, #e3f2fd, #ffffff);
      margin: 0;
      padding: 2rem;
    }
    h1 {
      font-size: 2rem;
    }
    .logout-btn {
      background: red;
      color: white;
      padding: 0.6rem 1.2rem;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .form-section {
      margin-top: 2rem;
      background: #f9f9f9;
      padding: 1.5rem;
      border-radius: 10px;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
    }
    input, button {
      padding: 0.5rem;
      margin: 0.5rem 0;
      width: 100%;
    }
    .dashboard-illustration {
      width: 250px;
      margin: 1rem auto;
      display: block;
    }
  </style>
</head>
<body>
  <img src="https://cdni.iconscout.com/illustration/premium/thumb/web-developer-doing-code-4358804-3618039.png" alt="Developer Illustration" class="dashboard-illustration">
  <h1>Welcome to Your Dashboard</h1>
  <button class="logout-btn" onclick="logout()">Logout</button>

  <div class="form-section">
    <h3>Notify Payment</h3>
    <input type="text" id="name" placeholder="Full Name" required />
    <input type="tel" id="phone" placeholder="Phone Number" required />
    <input type="text" id="amount" placeholder="Amount Paid (NGN)" required />
    <button onclick="submitPayment()">Submit Payment</button>
    <p id="submit-status"></p>
  </div>

  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      appId: "YOUR_APP_ID"
    };
    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const db = firebase.firestore();

    auth.onAuthStateChanged((user) => {
      if (!user) window.location.href = "login.html";
    });

    function logout() {
      auth.signOut();
    }

    async function submitPayment() {
      const name = document.getElementById('name').value;
      const phone = document.getElementById('phone').value;
      const amount = document.getElementById('amount').value;
      try {
        await db.collection("payments").add({
          name,
          phone,
          amount,
          timestamp: new Date(),
          status: "pending"
        });
        document.getElementById('submit-status').textContent = "✅ Payment submitted successfully!";

        // WhatsApp auto alert
        const message = `New Payment Notification:%0AName: ${name}%0APhone: ${phone}%0AAmount: NGN${amount}`;
        const whatsappLink = `https://wa.me/2349021335159?text=${message}`;
        window.open(whatsappLink, '_blank');
      } catch (err) {
        document.getElementById('submit-status').textContent = "❌ Error submitting: " + err.message;
      }
    }
  </script>
</body>
</html>
