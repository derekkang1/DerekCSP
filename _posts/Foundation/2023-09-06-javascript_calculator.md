---
toc: true
comments: false
layout: post
title: Calculator MD
description: In-Website Calculator
type: tangibles
courses: { compsci: {week: 2} }
permalink: jscalc
---

<style>
  .calculator-container { 
    width: 40vw; /* this width and height is specified for mobile devices by default */
    height: 80vh;
    margin: 0 auto;
  
    display: grid;
    grid-template-columns: repeat(4, 1fr); /* fr is a special unit; learn more here: https://css-tricks.com/introduction-fr-css-unit/  */
    grid-template-rows: 0.5fr repeat(5, 1fr);
    gap: 10px 10px;
  }

  .calculator-output {
    grid-column: span 4;
    padding: 0.25em;
    font-size: 20px;
    border: 5px solid white;
    display: flex;
    align-items: center;
    justify-content: center; /* Center text */
    color: white;
    background-color: black; /* Optional: background color for visibility */
  }

  .calculator-number,
  .calculator-operation,
  .calculator-clear,
  .calculator-equals {
    width: auto;
    height: auto;
    border-radius: 10px;
    background-color: #FFA300;
    color: black;
    border: 3px solid black;
    font-size: 1.5em;

    display: flex;
    justify-content: center;
    align-items: center;

    grid-column: span 1;
    grid-row: span 1;

    // Creates smooth animation effect
    transition: all 0.5s; 
  }

  .calculator-number:hover,
  .calculator-operation:hover,
  .calculator-clear:hover,
  .calculator-equals:hover {
    background-color: darkgrey; /* Change color on hover */
  }
</style>

<!-- Add a container for the animation -->
<div id="animation">
  <div class="calculator-container">
      <!-- Result -->
      <div class="calculator-output" id="output">0</div>
      <!-- Row 1 -->
      <div class="calculator-number">1</div>
      <div class="calculator-number">2</div>
      <div class="calculator-number">3</div>
      <div class="calculator-operation">+</div>
      <!-- Row 2 -->
      <div class="calculator-number">4</div>
      <div class="calculator-number">5</div>
      <div class="calculator-number">6</div>
      <div class="calculator-operation">-</div>
      <!-- Row 3 -->
      <div class="calculator-number">7</div>
      <div class="calculator-number">8</div>
      <div class="calculator-number">9</div>
      <div class="calculator-operation">*</div>
      <!-- Row 4 -->
      <div class="calculator-operation">√</div>
      <div class="calculator-operation">^</div>
      <div class="calculator-operation">1/x</div>
      <div class="calculator-operation">/</div>
      <!-- Row 5 -->
      <div class="calculator-clear">A/C</div>
      <div class="calculator-number">0</div>
      <div class="calculator-number">.</div>
      <div class="calculator-equals">=</div>
  </div>
</div>

<!-- JavaScript (JS) implementation of the calculator. -->
<script>
  // Initialize important variables to manage calculations
  var firstNumber = null;
  var operator = null;
  var nextReady = true;

  // Build objects containing key elements
  const output = document.getElementById("output");
  const numbers = document.querySelectorAll(".calculator-number");
  const operations = document.querySelectorAll(".calculator-operation");
  const clear = document.querySelectorAll(".calculator-clear");
  const equals = document.querySelectorAll(".calculator-equals");

  // Number buttons listener
  numbers.forEach(button => {
    button.addEventListener("click", function() {
      number(button.textContent);
    });
  });

  // Number action
  function number(value) {
    if (value != ".") {
      if (nextReady == true) {
        output.innerHTML = value;
        if (value != "0") {
          nextReady = false;
        }
      } else {
        output.innerHTML += value; // Concatenation for input
      }
    } else {
      if (output.innerHTML.indexOf(".") == -1) {
        output.innerHTML += value;
        nextReady = false;
      }
    }
  }

  // Operation buttons listener
  operations.forEach(button => {
    button.addEventListener("click", function() {
      operation(button.textContent);
    });
  });

  // Operator action
  function operation(choice) {
    if (firstNumber == null) {
      firstNumber = parseFloat(output.innerHTML);
      nextReady = true;
      operator = choice;
      return;
    }
    firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
    operator = choice;
    output.innerHTML = firstNumber.toString();
    nextReady = true;
  }

  // Calculator function
  function calculate(first, second) {
    let result = 0;
    switch (operator) {
      case "+":
        result = first + second;
        break;
      case "-":
        result = first - second;
        break;
      case "*":
        result = first * second;
        break;
      case "/":
        result = first / second;
        break;
      case "^":
        result = first ** second;
        break;
      case "√":
        result = Math.sqrt(first);
        break;
      case "1/x":
        result = 1 / first;
        break;
    }
    return result;
  }

  // Equals button listener
  equals.forEach(button => {
    button.addEventListener("click", function() {
      equal();
    });
  });

  // Equal action
  function equal() {
    firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
    output.innerHTML = firstNumber.toString();
    nextReady = true;
  }

  // Clear button listener
  clear.forEach(button => {
    button.addEventListener("click", function() {
      clearCalc();
    });
  });

  // A/C action
  function clearCalc() {
    firstNumber = null;
    output.innerHTML = "0";
    nextReady = true;
  }

  window.addEventListener("keydown", function(event) {
    handleKeyPress(event.key);
  });

  function handleKeyPress(key) {
    if (/^[0-9.]$/.test(key)) {
      number(key);
    } else if (key === "+" || key === "-" || key === "*" || key === "/" || key === "^" || key === "√" || key === "Enter") {
      operation(key);
    } else if (key === "Escape") {
      clearCalc();
    }
  }
</script>

<!-- Vanta animations -->
<script src="{{site.baseurl}}/assets/js/three.r119.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.halo.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.birds.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.net.min.js"></script>
<script src="{{site.baseurl}}/assets/js/vanta.rings.min.js"></script>
<script>
  // Setup Vanta scripts as functions
  var vantaInstances = {
    halo: VANTA.HALO,
    birds: VANTA.BIRDS,
    net: VANTA.NET,
    rings: VANTA.RINGS
  };
  // Obtain a random Vanta function
  var vantaInstance = vantaInstances[Object.keys(vantaInstances)[Math.floor(Math.random() * Object.keys(vantaInstances).length)]];
  // Run the animation
  vantaInstance({
    el: "#animation",
    mouseControls: true,
    touchControls: true,
    gyroControls: false
  });
</script>
