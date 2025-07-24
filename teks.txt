<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>HB Barbershop</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: url('IMG_2048.png') no-repeat center center fixed;
      background-size: cover;
      font-family: Arial, sans-serif;
      color: white;
      text-align: center;
    }
    .overlay {
      background: rgba(0, 0, 0, 0.6);
      padding: 40px 20px;
      min-height: 100vh;
    }
    .logo {
      max-width: 150px;
      margin-bottom: 20px;
    }
    h1 {
      font-size: 36px;
      margin-bottom: 10px;
    }
    .price {
      font-size: 20px;
      margin-bottom: 30px;
    }
    .btn {
      display: inline-block;
      background: #25d366;
      color: white;
      padding: 15px 25px;
      margin: 10px;
      border: none;
      border-radius: 5px;
      font-size: 18px;
      text-decoration: none;
      transition: background 0.3s ease;
    }
    .btn:hover {
      background: #1ebc59;
    }
    .payment {
      font-size: 18px;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <div class="overlay">
    <img src="IMG_2048.png" alt="HB Logo" class="logo" />
    <h1>HB Barbershop</h1>
    <p class="price">Harga Potong: Rp15.000 - Rp20.000</p>

    <a class="btn" href="https://wa.me/6289518371444?text=Halo%20saya%20ingin%20reservasi%20potong%20rambut">Reservasi via WhatsApp</a>

    <div class="payment">
      <p>Pembayaran via DANA:</p>
      <p><strong>0895-1837-1444</strong></p>
    </div>
  </div>
</body>
</html>
