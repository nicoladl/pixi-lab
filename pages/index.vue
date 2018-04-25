<template>
  <div class="app"></div>
</template>

<script>
export default {
  mounted () {

    // Create a new application
    const app = new PIXI.Application({
      width: window.innerWidth,
      height: window.innerHeight
    })

    // Display rectangle
    document.querySelector('.app').appendChild(app.view)
    app.stage.interactive = true


    let bg = PIXI.Texture.fromImage('/advice.jpg')
    let background = new PIXI.Sprite(bg)
    app.stage.addChild(background)

    let alphaFilter = new PIXI.filters.AlphaFilter(0.5)
    let blurFilter = new PIXI.filters.BlurFilter(10)
    // background.filters = [blurFilter, alphaFilter]

    let displacementSprite = PIXI.Sprite.fromImage('/4.png')
    displacementSprite.width = 100
    displacementSprite.height = app.screen.height
    let displacementFilter = new PIXI.filters.DisplacementFilter(displacementSprite)
    app.stage.addChild(displacementSprite)

    background.filters = [displacementFilter]

    app.stage
      .on('mousemove', onPointerMove)

    function onPointerMove(eventData) {

      let position = eventData.data.global.x
      setTimeout(() => {
        displacementSprite.position.x = position - 50
      }, 100)

      // displacementSprite.width = position
    }
  }
}
</script>

<style>
  canvas {
    margin: -10px;
  }
</style>

