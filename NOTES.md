# Course Notes

## Part 1. Welcome

### 1-2 - Brief history of WebGL and web graphics

WebGL, web api permettant de faire des visuels complexes basé sur openGl, utilisé dans et par de nombreux logiciels tels que blender, photoshop ...
OpenGL permet de standardiser le développement 

La version 1 de webGL (2011) est basée sur la verson OpenGL ES 2.0 (2007), conçu initialement pour les téléphones mobiles.

WebGl n'est pas une librairie, c'est un "rasterisation engine"

### Part 2. WebGL drawing fundamentals

### 1-1 - App boilerplate

Le contexte du canvas à choisir est 'webgl'.

```
  gl.enable(gl.SCISSOR_TEST)
  gl.viewport(0, 0, gl.drawingBufferWidth, gl.drawingBufferHeight)


  gl.clearColor(1, 0.2, 0.1, 1)
  gl.scissor(0, 0, gl.drawingBufferWidth / 2, gl.drawingBufferHeight)
  gl.clear(gl.COLOR_BUFFER_BIT)

  gl.clearColor(0.1, 0, 1, 1)
  gl.scissor(gl.drawingBufferWidth / 2, 0, gl.drawingBufferWidth / 2, gl.drawingBufferHeight)
  gl.clear(gl.COLOR_BUFFER_BIT)
```

