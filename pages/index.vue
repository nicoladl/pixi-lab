<template>
  <div class="app"></div>
</template>

<script>
import PixiPlugin from 'gsap'

export default {
  mounted () {

    // Create a new application
    const app = new PIXI.Application({
      width: window.innerWidth,
      height: window.innerHeight
    })

    app.renderer.view.style.position = "absolute"
    app.renderer.view.style.display = "block"
    app.renderer.autoResize = true
    app.renderer.resize(window.innerWidth, window.innerHeight)

    document.addEventListener('resize', function() {
      app.renderer.resize(window.innerWidth, window.innerHeight)
    })

    // Display rectangle
    document.querySelector('.app').appendChild(app.view)
    app.stage.interactive = true

    let bg = PIXI.Texture.fromImage('https://source.unsplash.com/CnXxQyaFrEU/1920x1280')
    let background = new PIXI.Sprite(bg)
    app.stage.addChild(background)

    let alphaFilter = new PIXI.filters.AlphaFilter(0.5)
    let blurFilter = new PIXI.filters.BlurFilter(10)
    // background.filters = [blurFilter, alphaFilter]

    let displacementSprite = PIXI.Sprite.fromImage('./gradient_small.png')
    displacementSprite.height = app.screen.height
    let displacementFilter = new PIXI.filters.DisplacementFilter(displacementSprite)
    app.stage.addChild(displacementSprite)

    background.filters = [displacementFilter]

    var prevEvent, currentEvent
    document.documentElement.onmousemove=function(event){
      currentEvent = event
    }

    app.stage
      .on('mouseout', onPointerLeave)
      .on('mousemove', onPointerMove)

    let timestamp
    let lastMouseX

    function normalize(val, max, min) {
      return (val - min) / (max - min)
    }

    function onPointerLeave(eventData) {
      displacementSprite.scale.x = 0
    }

    function onPointerMove(eventData) {

      if (timestamp === null) {
        lastMouseX = eventData.data.global.x
        return
      }

      let dx = eventData.data.global.x - lastMouseX
      lastMouseX = eventData.data.global.x

      // follow mouse position
      let position = eventData.data.global.x
      setTimeout(() => {
        displacementSprite.position.x = position - (displacementSprite.scale.x / 2)
      }, 250)
      // end

      let offset = dx*(-0.025)

      TweenMax.to(displacementSprite.scale, 0.5, { x: offset })
    }
  }
}
</script>

<style>
  canvas {
    margin: -1%;
  }
</style>


