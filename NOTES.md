# Course Notes

## Part 1. Welcome

### 1-2 - Brief history of WebGL and web graphics

WebGL est une web API (Application Programming Interface) permettant de faire des visuels complexes basé sur OpenGL. utilisé dans et par de nombreux logiciels tels que blender, photoshop ...
OpenGL(pour Open Graphic Library) est une API graphique qui propose des fonctions graphiques utilisable âr les développeur pour tracer des élements sur un écran.

OpenGL a été crée en 1992 par Silicon Graphic, une société américaine qui construisait des workstations dédiées aux domaines de l'infographie et de la 3D. De nos jours la sociéné n'existe plus mais le projet à été réccupéré par le consortium industriel "Khronos Group" en 2000. OpenGL est utilisé parce qu'il permet de standardiser le développement d'applications. Il existe plusieurs versions d'OpenGL, dont une appellée "OpenGL ES", sortie en 2003 et qui est une version allégée et qui a pour but de tourner sur des terminaux peu puissants et ayant peu de mémoire vives.

La version 1.0 de WebGL (2011) est basée sur la version OpenGL ES 2.0 (2007), conçu initialement pour les téléphones mobiles. 
<!-- WebGL n'est pas une librairie, c'est un "rasterisation engine" -->

### Part 2. WebGL drawing fundamentals

### 2-2 - App boilerplate

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


### 2-2 - Drawing a single point

Une application WebGL est découpé en deux parties : le côté javascript et le côté GPU.
Le travail du GPU peut être représenté par deux programmes appellés "Vertex Shader" et "Fragment Shader". Le procédé a donc pour but de transformer des données javascript en pixel à travers la pipeline de rendu webgl (pipeline rendering). Il y a d'autres étapes dans le processus (qui sont géré par OpenGL lui même) et ne nous laisse la main que sur les deux programes "vertex" et "fragment".

#### Vertex Shader

Le premier a être exécuté par le GPU est le Vertex Shader, qui a pour but de tranformer les coordonnées que le javascript va fournir en coordonnées qui vont être rendue dans le canvas webgl. 

Les coordonnées d'origines sont appellée "World Coordinates", tandis que les coordonnées finales sont appellées "Clip Coordinates". Le fait est que même si notre scene est en 3d, le rendu final à l'écran est une image en 2D.
Toutes les transformations de positions (rotation, position, ...) vont être réalisées durant cette étape Vertex.

#### Fragment Shader (a.k.a. pixel shader)
Le deuxième programme qui va être exécuté est le "Fragment" shader. Une fois les coordonnées convertie en position pixel dans l'image, le fragment va assigner la couleur de ce pixel. Cette couleur peut varier selon une source de lumière, des ombres, des textures, ...

#### Vertices

Les verticies sont basiquement des points qui vont êtres fourni par le javascript au vertex shader. Pour générer un triangle par exemple, il sera nécessaire de fournir 3 verticices au programme.

#### Rasterizer
...


#### Framebuffer
Framebuffer défini le "device screen". Cela signifie l'écren. Le framebuffer n'est pas obligatoirement rendu à l'écran, il peut être utilisé dans d'autres opérations pour générer des effets visuels en l'aditionnant à d'autres framebuffers par exemple.


#### Créer un shader

Pour utiliser un shader, il est nécessaire de le créer, que ce soit pour le vertex et pour le fragment.

On crée donc une fonction ***createShader(type, source)*** en lui faisant passer le type de shader que l'on souhaite créer et la source.

```
  const vertexShader = createShader(gl.VERTEX_SHADER, vertexShaderSource)
  const fragmentShader = createShader(gl.FRAGMENT_SHADER, fragmentShaderSource)
```

La fonction va donc créer le shader à partir de la methids ***gl.createShader()*** pour ensuite la compiler. Il est nécessaire de vérifier que la compilation soit un succès avant de la retourner. On utiulise la méthode gl.getShaderParameter(shader, gl.COMPILE_STATUS) pour vérifier cela. L'objet retournée est de type ***WEBGLShader*** et webgl n'aceppete que ce type.


#### Créer un program

Une fois nos deux shaders crées, il faut les lier. Pour cela nous devons créer un "program".
Nous devons également le compiler et vérifier que tout est ok. Dans ce cas là, on retourne un ***WebGLProgram*** Object.


#### Écrire un shader

Les shaders sont écrits dans un language spécifique appellé GLSL, proche du language C.

##### Vertex 

```
const vertexShaderSource = `
  attribute vec4 position;
  
  void main() {
    gl_Position = vec4(position);
  }
`
```

L'attribut "position" est fourni par le javascript. le shader va exécuter la fonction "main" et définir la variable ***gl_Position***.

##### Fragment 

```
const fragmentShaderSource = `
  void main() {
    gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);
  }
`
```

Du couté du fragment, le shader va également exécuter la fonction "main" et définir la variable ***gl_FragColor***.


#### Stocker les informations

##### Float32Array()

Pour stocker les positions, il est nécessaire de les stocker dans un tableau. Cependant on ne peut pas utiliser un tableau "classique", il est nécessaure de créer un ***new Float32Array()***, un tableau n'acceptant que les nombres (contrairement à un array classique qui peut stocker à la fois string, array, object, number, ...).
Il existe d'autres types de tableaux (ex: Float64Array, Int8Array) mais c'est majoritairement le Float32 qui sera utilisé. Cela peut servir pour stocker des positions mais égelment des valeurs de couleur par exemple.

##### WebGl Buffer

Nous avons besoin d'un "buffer" permettant de stocker nos positions et qui vont être fournis au GPU (vertex).

##### Vertices

Corrdonnées d'un point (x, y par exmeple pour de la 2D) stocké dans un Float32Array.


### 2-3 - WebGL Coordinate System

En WebGL, le centre de l'écran est représenté par les coordonnées [0,0], Les valeurs allant de -1 à 1 pour les bords de l'écran.

Dans ce cas, l'angle en haut à gauche de l'écran a pour coordonénes [-1,1], tandis que l'angle en bas à droite a pour coordonnées [1,-1].


### 2-4 - Adding points on mouse click

Dans le cas où l'on souhaite gérer aléatoirement les couleurs de nos carrés depuis le javascript, on va faire passer un attribut "color" qui sera tranmis au fragment shader. Sauf que le fragment shader ne peut pas avoir directement accès aux attributs, il faut qu'ils soivent transmis par le vertex. Pour cela on doit définir un "variyng" qui sera ensuite disponilbe dans le fragment.

```
  vertex shader : vertex.glsl
    const vertexShaderSource = `
      attribute vec4 position;
      attribute vec4 color; // 1. On réccupère la couleur dans le vertex

      varying vec4 vColor; // 2. On défini un varying vColor

      void main() {
        vColor = color; // 3. On défini la valeur du varying
        gl_Position = position;
        gl_PointSize = 20.0;
      }
    `
  
  fragment shader : fragment.glsl
    const fragmentShaderSource = `
      varying vec4 vColor; // 4. On récupère le varying fourni par le vertex shader
      void main() {
        gl_FragColor = vColor; // 5. On applique la couleur
      }
    `
```