<template>
  <div v-html="board"></div>
  <button :onclick="start">start</button>
</template>

<script>
const BOARD_SIZE = 10
const BOARD_PIXEL = 'o'
const FOOD_PIXEL = '@'

const UP = 0;
const RIGHT = 1;
const DOWN = 2;
const LEFT = 3;
const DIRECT_X = [-1, 0, 1, 0]
const DIRECT_Y = [0, 1, 0, -1]

export default {
  data() {
    return {
      board: "",
      count: 0,
      pos: {
        x: 0,
        y: 0,
      }
    }
  },
  methods: {
    draw() {
      let board_string = ""
      for(let x = 0; x < BOARD_SIZE; ++x) {
          for(let y = 0; y < BOARD_SIZE; ++y) {
            if(this.pos.x == x && this.pos.y == y) {
              board_string += FOOD_PIXEL
            } else {
              board_string += BOARD_PIXEL
            }
          }
          board_string += "<br/>"
      }
      this.board = board_string
    },
    isOutBound(x, y) {
        return !(x >= 0 && x < BOARD_SIZE && y >= 0 && y < BOARD_SIZE)
    },
    move(direct) {
        let next_pos = {x: this.pos.x + DIRECT_X[direct], y :this.pos.y + DIRECT_Y[direct]}
        if(this.isOutBound(next_pos.x, next_pos.y)){
            return
        } else {
            this.pos = next_pos
            this.draw()
        }
    },
    main() {
      // setInterval(this.draw, this.tickTime());
    },
    tickTime() {
      // Game speed increases as snake grows.
      var start = 800;
      // var end = 75;
      // return start + snake.length * (end - start) / grid.length;
      return start;
    },
    start() {
      this.main()
      this.draw()
    },
    async handleEvent(event) {
      switch (event.code) {
        case "KeyW":
        case "ArrowUp":
        //   console.log("up")
          this.move(UP)
          break;
        case "KeyS":
        case "ArrowDown":
        //   console.log("down")
          this.move(DOWN)
          break;   
        case "KeyA":
        case "ArrowLeft":
        //   console.log("left")
          this.move(LEFT)
          break;
        case "KeyD":
        case "ArrowRight":
        //   console.log("right")
          this.move(RIGHT)
          break;
      }
    }
  },
  // https://blog.csdn.net/qq_42415827/article/details/121144656?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-121144656-blog-125626225.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-121144656-blog-125626225.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=1
  mounted() {
      window.addEventListener('keydown', this.handleEvent)
  },
  beforeUnmount() {
      window.removeEventListener('keydown', this.handleEvent)
  },
}
</script>