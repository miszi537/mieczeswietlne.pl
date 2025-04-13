<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <title>Sklep z Mieczami Świetlnymi</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Orbitron&display=swap');

    body {
      font-family: 'Orbitron', sans-serif;
      background-color: #000;
      color: #00ffff;
      margin: 0;
      padding: 0;
      text-align: center;
    }
    header {
      background: #111;
      padding: 20px 0;
      box-shadow: 0 0 10px #0ff;
    }
    h1 {
      margin: 0;
      font-size: 2.5em;
    }
    .products {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      margin: 30px auto;
      gap: 40px;
    }
    .product {
      background-color: #111;
      border: 2px solid #0ff;
      border-radius: 10px;
      width: 280px;
      padding: 20px;
      box-shadow: 0 0 15px #0ff;
    }
    .product img {
      width: 100%;
      border-radius: 8px;
    }
    .price {
      font-size: 1.2em;
      margin: 10px 0;
    }
    button {
      background-color: #00ffff;
      color: #000;
      border: none;
      padding: 10px 20px;
      border-radius: 5px;
      cursor: pointer;
      margin-top: 10px;
    }
    button:hover {
      background-color: #00aaaa;
    }
    #cart {
      margin: 40px auto;
      width: 90%;
      max-width: 500px;
      padding: 20px;
      background-color: #111;
      border: 2px solid #0ff;
      border-radius: 10px;
    }
  </style>
</head>
<body>

  <header>
    <h1>Sklep z Mieczami Świetlnymi</h1>
  </header>

  <div class="products">
    <div class="product">
      <img src="https://upload.wikimedia.org/wikipedia/commons/0/0b/Lightsaber_Blue.svg" alt="Srebrny miecz">
      <h2>Srebrny Miecz</h2>
      <p class="price">499 zł</p>
      <button onclick="addToCart('Srebrny Miecz', 499)">Dodaj do koszyka</button>
    </div>

    <div class="product">
      <img src="https://upload.wikimedia.org/wikipedia/commons/6/6a/Lightsaber_red.svg" alt="Czarny miecz">
      <h2>Czarny Miecz</h2>
      <p class="price">549 zł</p>
      <button onclick="addToCart('Czarny Miecz', 549)">Dodaj do koszyka</button>
    </div>
  </div>

  <div id="cart">
    <h2>Twój koszyk</h2>
    <ul id="cart-items" style="list-style: none; padding-left: 0;"></ul>
    <p id="total">Suma: 0 zł</p>
    <div id="paypal-button-container"></div>
  </div>

  <script src="https://www.paypal.com/sdk/js?client-id=AXGarn8Rl5epcYtvghsFHtTWTpxjcNYb9U8fc_J_dKFVSd1elVp9nKFEqmd3GQrARgObZ1XyKM0pODRO&currency=PLN"></script>
  <script>
    let cart = [];
    function addToCart(name, price) {
      cart.push({ name, price });
      updateCart();
    }

    function updateCart() {
      const list = document.getElementById('cart-items');
      const total = document.getElementById('total');
      list.innerHTML = '';
      let sum = 0;
      cart.forEach(item => {
        const li = document.createElement('li');
        li.textContent = `${item.name} - ${item.price} zł`;
        list.appendChild(li);
        sum += item.price;
      });
      total.textContent = `Suma: ${sum} zł`;
    }

    paypal.Buttons({
      createOrder: function(data, actions) {
        let total = cart.reduce((sum, item) => sum + item.price, 0);
        return actions.order.create({
          purchase_units: [{ amount: { value: total.toString(), currency_code: 'PLN' } }]
        });
      },
      onApprove: function(data, actions) {
        return actions.order.capture().then(function(details) {
          alert('Dziękujemy, ' + details.payer.name.given_name + '! Płatność zakończona.');
          cart = [];
          updateCart();
        });
      }
    }).render('#paypal-button-container');
  </script>

</body>
</html>
