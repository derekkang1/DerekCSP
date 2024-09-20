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
        <button id="buy-farm">Buy Farm (250 cookies)</button>
        <div id="farm-info">Farms: <span id="farms">0</span></div>
    </div>
</body>

<script>
let cookieCount = 0;
let autoClickers = 0;
let grandmas = 0;
let farms = 0;
let cursorPrice = 15;
let grandmaPrice = 100; 
let farmPrice = 250; // Make sure this matches your button price

const cookieCountElement = document.getElementById("cookie-count");
const autoClickerElement = document.getElementById("auto-clickers");
const grandmaElement = document.getElementById("grandmas");
const farmElement = document.getElementById("farms"); // Corrected to "farms"
const buyAutoClickerButton = document.getElementById("buy-auto-clicker");
const buyGrandmaButton = document.getElementById("buy-grandma");
const buyFarmButton = document.getElementById("buy-farm");

function playPointSound() {
    const pointSound = document.getElementById("pointSound");
    pointSound.play();
}
function updateCookieCount() {
    cookieCountElement.textContent = cookieCount;
}
function updateAutoClickers() {
    autoClickerElement.textContent = autoClickers;
}
function updateGrandmas() {
    grandmaElement.textContent = grandmas;
}
function updateFarms() { // Define the function to update farms
    farmElement.textContent = farms; // Use 'farms' variable
}
function updateCursorPrice() {
    buyAutoClickerButton.textContent = `Buy Cursor (${cursorPrice} cookies)`; 
}
function updateGrandmaButton() {
    buyGrandmaButton.textContent = `Buy Grandma (${grandmaPrice} cookies)`;
}
function updateFarmButton() {
    buyFarmButton.textContent = `Buy Farm (${farmPrice} cookies)`;
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

// Buy farm event
buyFarmButton.addEventListener("click", () => {
    if (cookieCount >= farmPrice) {
        cookieCount -= farmPrice;
        farms++;
        farmPrice = Math.floor(farmPrice * 1.5); // Increase price for next purchase
        updateCookieCount();
        updateFarms(); // Call the newly defined function
        updateFarmButton(); // Update the button text with the new price
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

// Farm functionality: adds cookies every second
setInterval(() => {
    if (farms > 0) {
        cookieCount += 10 * farms; // 10 cookies per farm
        updateCookieCount();
    }
}, 1000);

// Initial display
updateCookieCount();
updateAutoClickers();
updateGrandmas();
updateFarms(); // Call the function to update farms
updateCursorPrice();
updateGrandmaButton();
updateFarmButton(); // Initial update for farm button
</script>