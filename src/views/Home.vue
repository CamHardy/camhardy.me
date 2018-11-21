<template>
  <div class="home hero is-light is-fullheight-with-navbar bg-img">
    <vue-p5 @sketch="sketch"></vue-p5>
    <div class="hero-body has-text-centered">
      <div class="container">
        <div class="title section is-paddingless">Hello.</div>
        <div class="is-divider is-paddingless is-mobile" style="margin: auto; width: 50%"></div>
        <div class="subtitle section">I'm a Calgary-based software developer with a passion for graphics and design.</div>
        <div>
        <nav class="level is-mobile">
          <div class="level-item"></div>
          <div class="level-item has-text-centered">
            <a href="https://www.linkedin.com/in/cameron-hardy-26845a157/">
              <i class="fab fa-linkedin"></i>
              <p class="heading">LinkedIn</p>
            </a>
          </div>
          <div class="level-item has-text-centered">
            <a href="https://github.com/CamHardy">
              <i class="fab fa-github"></i>
              <p class="heading">Github</p>
            </a>
          </div>
          <div class="level-item"></div>
        </nav>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
  .bg-img {
    background-image: url('../assets/bg.png');
    background-position: center center;
    background-repeat:  no-repeat;
    background-attachment: fixed;
    background-size:  cover;
    background-color: #999;
  }
  canvas {
    position: absolute;
  }
</style>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import VueP5 from 'vue-p5'

export default {
  name: 'home',
  components: {
    HelloWorld,
    "vue-p5": VueP5
  },
  methods: {
      sketch(sk) {
          let bgColor = 245;
          let triColors = [252, 251, 249, 248, 241, 240];
          let col_num = 0;
          // since we're using triangles there aren't really rows in the traditional sense
          // this is the count of triangles stacked in a column
          let row_num = 0;
          let triSize = 22;
          let triGap = 2;

          sk.setup = () => {
              // set up canvas
              sk.createCanvas(sk.windowWidth, sk.windowHeight - 75);
              sk.background(bgColor);
              sk.frameRate(1);

              // set up draw variables
              col_num = sk.floor(sk.width / (triSize + triGap)) - 5;
              row_num = sk.floor(sk.height / (triSize + triGap)) + 1;
              drawBackground();
          };

          sk.draw = () => {
              //TODO: create an array of colors (grey tones)
              //TODO: create an array of triangles to cover the screen
              //TODO: each draw call, random triangles will be marked for update
              //TODO: triangles marked for update choose a random new color
              //TODO: draw all triangles from array
          };

          sk.windowResized = () => {
              sk.resizeCanvas(sk.windowWidth, sk.windowHeight - 75);
              sk.background(bgColor);
              sk.frameRate(1);

              // set up draw variables
              col_num = sk.floor(sk.width / (triSize + triGap));
              row_num = sk.floor(sk.height / (triSize + triGap)) + 1;
              drawBackground();
          };

          function drawBackground() {
              for (let i = -triSize/4; i < col_num; i++) {
                  let rowInv = i % 2;
                  for (let j = -triSize/4; j < row_num; j++) {
                      let oi = i * (triSize + 2 * triGap);
                      let oj = j * (triSize + triGap);
                      drawTriangle(oi, oj, getColor());
                      drawTriangle(oi + (triSize + triGap), oj + ((triSize + triGap) / 2), getColor(), true);
                  }
              }
          }

          // x, y: draw position
          // color: draw color
          // inv: inverted triangles 'point' to the left
          function drawTriangle(x, y, color, inv = false) {
              sk.noStroke();
              sk.fill(color);
              sk.triangle(  x, y,                                                   // p1
                            inv ? x - triSize : x + triSize, y + (triSize / 2),     // p2
                            x, y + triSize   );                                     // p3
          }

          // return random color from triColors[]
          function getColor() {
              return triColors[(Math.floor(Math.random() * triColors.length))];
          }
      }
  }
}
</script>
