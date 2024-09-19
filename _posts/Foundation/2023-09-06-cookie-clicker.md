---
toc: true
comments: false
layout: post
type: tangibles
title: Cookie Clicker!
permalink: /cookieclicker
---

<style>
    body {
        text-align: center;
        background-color: #f7f7f7;
        margin: 0;
        padding: 0;
    }

    .container {
        margin-top: 100px;
    }

    #cookie-container {
        margin: 20px;
    }

    #cookie {
        width: 150px;
        cursor: pointer;
        transition: transform 0.1s ease;
    }

    #cookie:active {
        transform: scale(0.9);
    }

    #counter {
        font-size: 24px;
        margin: 20px;
    }

    button {
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
        background-color: #4CAF50;
        color: white;
        border: none;
        border-radius: 5px;
        margin: 10px;
    }

    button:disabled {
        background-color: grey;
    }

    #auto-clicker-info, #grandma-info {
        font-size: 18px;
        margin: 10px;
    }

    #auto-clicker-price, #grandma-price {
        font-size: 18px;
        margin: 10px;
    }
</style>

<audio id="pointSound" src="{{site.baseurl}}/audio/cookie.mp3" preload="auto"></audio>

<body>
    <div class="container">
        <div id="cookie-container">
            <img id="cookie" src="{{site.baseurl}}/images/cookie.png" alt="Cookie">
        </div>
        <div id="counter">Cookies: <span id="cookie-count">0</span></div>
        <button id="buy-auto-clicker">Buy Cursor (15 cookies)</button>
        <div id="auto-clicker-info">Cursors: <span id="auto-clickers">0</span></div>
        <button id="buy-grandma">Buy Grandma (100 cookies)</button>
        <div id="grandma-info">Grandmas: <span id="grandmas">0</span></div>
    </div>
</body>

<script>
    let cookieCount = 0;
    let autoClickers = 0;
    let grandmas = 0;
    let cursorPrice = 15; // Initial price of the cursor
    let grandmaPrice = 100; // Initial price of grandma

    const cookieCountElement = document.getElementById("cookie-count");
    const autoClickerElement = document.getElementById("auto-clickers");
    const grandmaElement = document.getElementById("grandmas");
    const buyAutoClickerButton = document.getElementById("buy-auto-clicker");
    const buyGrandmaButton = document.getElementById("buy-grandma");

    function playPointSound() {
        const pointSound = document.getElementById("pointSound");
        pointSound.play();
    }

    // Function to update cookie count display
    function updateCookieCount() {
        cookieCountElement.textContent = cookieCount;
    }

    // Function to update auto-clicker count display
    function updateAutoClickers() {
        autoClickerElement.textContent = autoClickers;
    }

    // Function to update grandma count display
    function updateGrandmas() {
        grandmaElement.textContent = grandmas;
    }

    // Function to update cursor price display
    function updateCursorPrice() {
        buyAutoClickerButton.textContent = `Buy Cursor (${cursorPrice} cookies)`; // Update button text
    }

    // Function to update grandma button text with price
    function updateGrandmaButton() {
        buyGrandmaButton.textContent = `Buy Grandma (${grandmaPrice} cookies)`;
    }

    // Cookie click event
    document.getElementById("cookie").addEventListener("click", () => {
        cookieCount++;
        updateCookieCount();
        playPointSound();
    });

    // Buy auto-clicker event
    buyAutoClickerButton.addEventListener("click", () => {
        if (cookieCount >= cursorPrice) {
            cookieCount -= cursorPrice;
            autoClickers++;
            cursorPrice = Math.floor(cursorPrice * 1.5); // Increase price for next purchase
            updateCookieCount();
            updateAutoClickers();
            updateCursorPrice(); // Update the displayed cursor price and button text
        }
    });

    // Buy grandma event
    buyGrandmaButton.addEventListener("click", () => {
        if (cookieCount >= grandmaPrice) {
            cookieCount -= grandmaPrice;
            grandmas++;
            grandmaPrice = Math.floor(grandmaPrice * 1.5); // Increase price for next purchase
            updateCookieCount();
            updateGrandmas();
            updateGrandmaButton(); // Update the button text with the new price
        }
    });

    // Auto-clicker functionality
    setInterval(() => {
        if (autoClickers > 0) {
            cookieCount += autoClickers;
            updateCookieCount();
        }
    }, 500);

    // Grandma functionality: adds cookies every second
    setInterval(() => {
        if (grandmas > 0) {
            cookieCount += 5 * grandmas; // 5 cookies per grandma
            updateCookieCount();
        }
    }, 1000);

    // Initial display
    updateCookieCount();
    updateAutoClickers();
    updateGrandmas();
    updateCursorPrice();
    updateGrandmaButton(); // Initial update for grandma button
</script>
