<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Noel Shop - Mua Sắm Giáng Sinh</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: #f0f0f0 url('https://i.imgur.com/MkkU6wP.jpg') no-repeat center center fixed;
            background-size: cover;
        }
        header {
            background-color: rgba(211, 47, 47, 0.9);
            color: white;
            padding: 10px 20px;
            text-align: center;
            width: 100%;
            position: fixed;
            top: 0;
            left: 0;
            z-index: 100;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        header .home-icon {
            margin-left: 10px;
            cursor: pointer;
        }
        header .search-bar {
            margin-right: 20px;
            flex-grow: 1;
            display: flex;
            justify-content: flex-end;
        }
        header .search-bar input {
            padding: 10px;
            width: 50%;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        #sidebar {
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            width: 80px;
            height: calc(100vh - 70px); /* Giảm trừ chiều cao của header */
            position: fixed;
            top: 70px; /* Đặt dưới header */
            left: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            padding-top: 20px;
        }
        #sidebar img {
            width: 40px;
            height: 40px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        #sidebar img:hover {
            transform: scale(1.1);
        }
        .products {
            margin: 120px 0 20px 100px;
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 20px;
        }
        .product-card {
            background-color: white;
            border: 1px solid #ccc;
            border-radius: 8px;
            padding: 10px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .product-card img {
            max-width: 100%;
            border-radius: 8px;
        }
        .product-card button {
            margin-top: 10px;
            padding: 10px;
            background-color: #d32f2f;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .product-card button:hover {
            background-color: #b71c1c;
        }
        #snow {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 10;
        }
    </style>
</head>
<body>
    <canvas id="snow"></canvas>

    <header>
        <img src="https://i.imgur.com/5C2v2KQ.png" alt="Home" class="home-icon" onclick="goToPage('home')">
        <h1>🎅 Noel Shop - Chào Mừng Giáng Sinh 🎄</h1>
        <div class="search-bar">
            <input type="text" id="searchInput" placeholder="Tìm kiếm sản phẩm..." oninput="searchProducts()">
        </div>
    </header>

    <div id="sidebar">
        <img src="https://i.imgur.com/hboKADw.png" alt="Cart" onclick="goToPage('cart')">
        <img src="https://i.imgur.com/y2gVcBQ.png" alt="Promo" onclick="goToPage('promo')">
    </div>

    <main>
        <div class="products" id="productContainer">
            <div class="product-card">
                <img src="https://i.imgur.com/UY7MeWV.jpg" alt="Ông già Noel">
                <h3>Ông già Noel</h3>
                <p>Giá: 300.000đ</p>
                <button onclick="addToCart('Ông già Noel', 300000)">Mua Ngay</button>
            </div>
            <div class="product-card">
                <img src="https://i.imgur.com/1b6zYxZ.jpg" alt="Cây thông Noel">
                <h3>Cây thông Noel</h3>
                <p>Giá: 500.000đ</p>
                <button onclick="addToCart('Cây thông Noel', 500000)">Mua Ngay</button>
            </div>
            <div class="product-card">
                <img src="https://i.imgur.com/kMlqkS8.jpg" alt="Vòng hoa Giáng Sinh">
                <h3>Vòng hoa Giáng Sinh</h3>
                <p>Giá: 150.000đ</p>
                <button onclick="addToCart('Vòng hoa Giáng Sinh', 150000)">Mua Ngay</button>
            </div>
        </div>
    </main>

    <script>
        // Hiệu ứng tuyết rơi
        const snowCanvas = document.getElementById('snow');
        const ctx = snowCanvas.getContext('2d');
        const snowflakes = [];

        function resizeCanvas() {
            snowCanvas.width = window.innerWidth;
            snowCanvas.height = window.innerHeight;
        }

        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        function createSnowflake() {
            return {
                x: Math.random() * snowCanvas.width,
                y: Math.random() * snowCanvas.height,
                radius: Math.random() * 3 + 1,
                speed: Math.random() * 1 + 0.5
            };
        }

        function updateSnowflakes() {
            ctx.clearRect(0, 0, snowCanvas.width, snowCanvas.height);
            snowflakes.forEach(snowflake => {
                snowflake.y += snowflake.speed;
                if (snowflake.y > snowCanvas.height) {
                    snowflake.y = -snowflake.radius;
                }
                ctx.beginPath();
                ctx.arc(snowflake.x, snowflake.y, snowflake.radius, 0, Math.PI * 2);
                ctx.fillStyle = 'white';
                ctx.fill();
            });
            requestAnimationFrame(updateSnowflakes);
        }

        for (let i = 0; i < 150; i++) {
            snowflakes.push(createSnowflake());
        }
        updateSnowflakes();

        // Chuyển hướng trang
        function goToPage(page) {
            if (page === 'home') {
                window.location.href = '#';
            } else if (page === 'cart') {
                alert("Đi đến trang giỏ hàng!");
            } else if (page === 'promo') {
                alert("Đi đến trang quảng bá!");
            }
        }

        // Thêm vào giỏ hàng
        function addToCart(productName, price) {
            alert(`🎁 Bạn vừa mua ${productName} với giá ${price.toLocaleString()}đ!`);
        }

        // Tìm kiếm sản phẩm
        function searchProducts() {
            const input = document.getElementById("searchInput").value.toLowerCase();
            const products = document.querySelectorAll(".product-card");

            products.forEach(product => {
                const title = product.querySelector("h3").textContent.toLowerCase();
                product.style.display = title.includes(input) ? "block" : "none";
            });
        }
    </script>
</body>
</html>
