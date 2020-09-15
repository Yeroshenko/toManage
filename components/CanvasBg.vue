<template>
  <canvas ref="canvas" class="canvas-bg"></canvas>
</template>

<script>
export default {
  data() {
    return {
      light: {
        x: 160,
        y: 200
      },

      colors: ['#1F74FF', '#46C93A', '#FF4757'],
      boxes: []
    }
  },
  mounted() {
    this.ctx = this.$refs.canvas.getContext('2d')

    this.draw()
    this.resize()

    while (this.boxes.length < 28) {
      this.boxes.push(
        new this.Box(this.$refs.canvas, this.ctx, this.colors, this.light)
      )
    }
    window.addEventListener('resize', this.resize)
    window.addEventListener('mousemove', this.mousemove)
  },

  beforeDestroy() {
    window.removeEventListener('resize', this.resize)
    window.removeEventListener('mousemove', this.mousemove)
  },
  methods: {
    resize() {
      const box = this.$refs.canvas.getBoundingClientRect()

      this.$refs.canvas.width = box.width
      this.$refs.canvas.height = box.height
    },

    mousemove(e) {
      this.light.x = e.clientX
      this.light.y = e.clientY
    },

    drawLight() {
      this.ctx.beginPath()
      this.ctx.arc(this.light.x, this.light.y, 1000, 0, 2 * Math.PI)
      let gradient = this.ctx.createRadialGradient(
        this.light.x,
        this.light.y,
        0,
        this.light.x,
        this.light.y,
        1000
      )
      gradient.addColorStop(0, '#3b4654')
      gradient.addColorStop(1, '#2c343f')

      this.ctx.fillStyle = gradient
      this.ctx.fill()
      this.ctx.beginPath()

      gradient = this.ctx.createRadialGradient(
        this.light.x,
        this.light.y,
        0,
        this.light.x,
        this.light.y,
        5
      )
      gradient.addColorStop(0, '#fff')
      gradient.addColorStop(1, '#3b4654')
      this.ctx.fillStyle = gradient
      this.ctx.fill()
    },

    Box(canvas, ctx, colors, light) {
      this.canvas = canvas
      this.ctx = ctx
      this.colors = colors
      this.light = light

      this.half_size = Math.floor(Math.random() * 50 + 1)
      this.x = Math.floor(Math.random() * this.canvas.width + 1)
      this.y = Math.floor(Math.random() * this.canvas.height + 1)
      this.r = Math.random() * Math.PI
      this.shadow_length = 2000
      this.color = this.colors[Math.floor(Math.random() * this.colors.length)]

      this.getDots = function () {
        const full = (Math.PI * 2) / 4

        const p1 = {
          x: this.x + this.half_size * Math.sin(this.r),
          y: this.y + this.half_size * Math.cos(this.r)
        }
        const p2 = {
          x: this.x + this.half_size * Math.sin(this.r + full),
          y: this.y + this.half_size * Math.cos(this.r + full)
        }
        const p3 = {
          x: this.x + this.half_size * Math.sin(this.r + full * 2),
          y: this.y + this.half_size * Math.cos(this.r + full * 2)
        }
        const p4 = {
          x: this.x + this.half_size * Math.sin(this.r + full * 3),
          y: this.y + this.half_size * Math.cos(this.r + full * 3)
        }

        return { p1, p2, p3, p4 }
      }
      this.rotate = function () {
        const speed = (60 - this.half_size) / 20

        this.r += speed * 0.002
        this.x += speed
        this.y += speed
      }
      this.draw = function () {
        const dots = this.getDots()

        this.ctx.beginPath()
        this.ctx.moveTo(dots.p1.x, dots.p1.y)
        this.ctx.lineTo(dots.p2.x, dots.p2.y)
        this.ctx.lineTo(dots.p3.x, dots.p3.y)
        this.ctx.lineTo(dots.p4.x, dots.p4.y)
        this.ctx.fillStyle = this.color
        this.ctx.fill()

        if (this.y - this.half_size > this.canvas.height) {
          this.y -= this.canvas.height + 100
        }
        if (this.x - this.half_size > this.canvas.width) {
          this.x -= this.canvas.width + 100
        }
      }
      this.drawShadow = function () {
        const dots = this.getDots()
        const angles = []
        const points = []

        for (const dot in dots) {
          const angle = Math.atan2(
            this.light.y - dots[dot].y,
            this.light.x - dots[dot].x
          )
          const endX =
            dots[dot].x + this.shadow_length * Math.sin(-angle - Math.PI / 2)
          const endY =
            dots[dot].y + this.shadow_length * Math.cos(-angle - Math.PI / 2)
          angles.push(angle)
          points.push({
            endX,
            endY,
            startX: dots[dot].x,
            startY: dots[dot].y
          })
        }

        for (let i = points.length - 1; i >= 0; i--) {
          const n = i === 3 ? 0 : i + 1
          this.ctx.beginPath()
          this.ctx.moveTo(points[i].startX, points[i].startY)
          this.ctx.lineTo(points[n].startX, points[n].startY)
          this.ctx.lineTo(points[n].endX, points[n].endY)
          this.ctx.lineTo(points[i].endX, points[i].endY)
          this.ctx.fillStyle = '#2c343f'
          this.ctx.fill()
        }
      }
    },

    draw() {
      this.ctx.clearRect(
        0,
        0,
        this.$refs.canvas.width,
        this.$refs.canvas.height
      )
      this.drawLight()

      for (let i = 0; i < this.boxes.length; i++) {
        this.boxes[i].rotate()
        this.boxes[i].drawShadow()
      }
      for (let i = 0; i < this.boxes.length; i++) {
        this.collisionDetection(i)
        this.boxes[i].draw()
      }
      requestAnimationFrame(this.draw)
    },

    collisionDetection(b) {
      for (let i = this.boxes.length - 1; i >= 0; i--) {
        if (i !== b) {
          const dx =
            this.boxes[b].x +
            this.boxes[b].half_size -
            (this.boxes[i].x + this.boxes[i].half_size)
          const dy =
            this.boxes[b].y +
            this.boxes[b].half_size -
            (this.boxes[i].y + this.boxes[i].half_size)
          const d = Math.sqrt(dx * dx + dy * dy)
          if (d < this.boxes[b].half_size + this.boxes[i].half_size) {
            this.boxes[b].half_size =
              this.boxes[b].half_size > 1 ? (this.boxes[b].half_size -= 1) : 1
            this.boxes[i].half_size =
              this.boxes[i].half_size > 1 ? (this.boxes[i].half_size -= 1) : 1
          }
        }
      }
    }
  }
}
</script>

<style lang="sass">
.canvas-bg
  position: absolute
  top: 0
  left: 0
  right: 0
  bottom: 0
  background-color: #2c343f
  width: 100%
  height: 100%
  z-index: -1
  cursor: pointer
</style>
