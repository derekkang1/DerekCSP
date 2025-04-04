---
layout: base
title: Student Home 
image: /images/mario_animation.png
hide: true
description: Home Page
menu: nav/index.html
---

<!-- Liquid:  statements -->

<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}  
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %} 

<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, 
        define how HTML elements look 
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  body {
    text-align: center;
  }
  .container {
    display: flex; 
    flex-direction: column; 
    align-items: center; /* Center items horizontally */
    margin: 0 auto;
  }
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
</style>

<!--- Embedded executable code--->
<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.direction = "right";
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      if(this.direction == "right"){
        this.animate(this.obj["Walk"], 3);
      }else if(this.direction == "left"){
        this.animate(this.obj["WalkL"], -3);
      }
    }

    startRunning() {
      this.stopAnimate();
      if(this.direction == "right"){
        this.animate(this.obj["Run1"], 6);
      }else if(this.direction == "left"){
        this.animate(this.obj["Run1L"], -6);
      }
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      if(this.direction == "right"){
        this.animate(this.obj["Cheer"], 0);
      }else if(this.direction == "left"){
        this.animate(this.obj["CheerL"], 0);
      }

      
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.direction = "right";
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startRunning();
        } else if (mario.currentSpeed === 6) {
          mario.startResting();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.direction = "left";
          mario.startWalking();
        } else if (mario.currentSpeed === -3) {
          mario.startRunning();
        } else if (mario.currentSpeed === -6) {
          mario.startResting();
        }
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${0.2 * scale})`;
    mario.startResting();
  });
</script>


<div class="container">
    <img src="{{site.baseurl}}/images/welcome.png">
    <br>
    <h1>Visit these cool parts of my blog!</h1>
    <div>
        <button onclick="window.location.href='{{site.baseurl}}/miniproject/home';">My Gaming Blog!</button>
    </div>
    <div>
        <button onclick="window.location.href='{{site.baseurl}}/snakegame';">Snake Game!</button>
    </div>
    <div>
        <button onclick="window.location.href='{{site.baseurl}}/cookieclicker';">Cookie Clicker!</button>
    </div>
    <div>
        <button onclick="window.location.href='{{site.baseurl}}/jscalc';">Calculator!</button>
    </div>
    <br>
    <br>
    <br>
    <img src="{{site.baseurl}}/images/cartlogo.png" width="180" height="92" />
    <h3>Check out my robotics team!</h3>
    <img src="{{site.baseurl}}/images/cart_robot.png" width="490" height="390" />
    <div>
        <button onclick="window.open('https://cartrobotics.org/');">Visit the team website</button>
    </div>
    <br>
    <br>
    <div>
        <h3>Join Del Norte SciOly!</h3>
        <img src="{{site.baseurl}}/images/dnscioly.png" width="100" height="100" />
    </div>
    <div>
        <button onclick="window.open('https://www.youtube.com/watch?v=dQw4w9WgXcQ');">Click Me!</button>
    </div>
    <div>
        <a href="https://docs.google.com/presentation/d/1o15b7_oeV8J7zY2rmnInHFQiUcuF7D9J3HM5Up-jdFE/edit?usp=sharing">Interest Meeting Presentation</a>
    </div>
    <div>
        <a href="https://forms.gle/xnwPbusonfWDk9za6">Register for the 2024-25 Season Here!</a>
        <p>Registration will be closing on September 8th at 11:59 PM.</p>
    </div>
</div>