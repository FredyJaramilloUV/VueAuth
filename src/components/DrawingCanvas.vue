<template>
  <canvas
    ref="canvasRef"
    width="600"
    height="400"
    tabindex="0"
    class="drawing-canvas"
    @mousedown="onMouseDown"
    @mouseup="onMouseUp"
    @mousemove="onMouseMove"
  ></canvas>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const canvasRef = ref(null)
const ctx = ref(null)

const cursor = ref({ x: 100, y: 100 })
const size = 5

let pressedKey = null
let moveInterval = null
const speed = 2

let drawingMode = 'circle' // puede ser 'circle', 'line', 'select', 'erase'
let isDrawing = false
let startX = 0
let startY = 0

function drawCursor() {
  if (!ctx.value) return
  ctx.value.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height)
  // Dibujar otros elementos (opcional)
  // Dibujar el cursor
  ctx.value.beginPath()
  ctx.value.arc(cursor.value.x, cursor.value.y, size, 0, Math.PI * 2)
  ctx.value.fillStyle = 'red'
  ctx.value.fill()
  ctx.value.closePath()
}

function moveCursor(key) {
  switch (key) {
    case 'ArrowRight': cursor.value.x += speed; break
    case 'ArrowLeft':  cursor.value.x -= speed; break
    case 'ArrowUp':    cursor.value.y -= speed; break
    case 'ArrowDown':  cursor.value.y += speed; break
  }
  drawCursor()
}

function onKeyDown(e) {
  if (['ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight'].includes(e.key)) {
    e.preventDefault()
    if (!moveInterval) {
      pressedKey = e.key
      moveInterval = setInterval(() => moveCursor(pressedKey), 16)
    }
  }

  if (e.key === 'c') drawingMode = 'circle'
  if (e.key === 'l') drawingMode = 'line'
  if (e.key === 's') drawingMode = 'select'
  if (e.key === 'e') drawingMode = 'erase'
  if (e.key === 'Enter') performAction()
}

function onKeyUp(e) {
  if (e.key === pressedKey) {
    clearInterval(moveInterval)
    moveInterval = null
    pressedKey = null
  }
}

function performAction() {
  const x = cursor.value.x
  const y = cursor.value.y
  if (!ctx.value) return

  if (drawingMode === 'circle') {
    ctx.value.beginPath()
    ctx.value.arc(x, y, 20, 0, Math.PI * 2)
    ctx.value.strokeStyle = 'blue'
    ctx.value.stroke()
    ctx.value.closePath()
  }

  if (drawingMode === 'line') {
    ctx.value.beginPath()
    ctx.value.moveTo(startX, startY)
    ctx.value.lineTo(x, y)
    ctx.value.strokeStyle = 'green'
    ctx.value.stroke()
    ctx.value.closePath()
  }

  if (drawingMode === 'erase') {
    ctx.value.clearRect(x - 10, y - 10, 20, 20)
  }

  if (drawingMode === 'select') {
    ctx.value.beginPath()
    ctx.value.rect(x - 20, y - 20, 40, 40)
    ctx.value.strokeStyle = 'orange'
    ctx.value.stroke()
    ctx.value.closePath()
  }

  // Guardar el punto inicial para líneas
  startX = x
  startY = y
}

onMounted(() => {
  ctx.value = canvasRef.value.getContext('2d')
  drawCursor()
  window.addEventListener('keydown', onKeyDown)
  window.addEventListener('keyup', onKeyUp)
})

onBeforeUnmount(() => {
  window.removeEventListener('keydown', onKeyDown)
  window.removeEventListener('keyup', onKeyUp)
})
</script>

<style scoped>
.drawing-canvas {
  border: 1px solid black;
  outline: none;
  background-color: #fff;
}
</style>
