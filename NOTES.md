# Course Notes

## Part 1. Welcome

### 1-1 - Intro
Contextes : 2d, webgl, webgl2, gpu

« 2d » est le plus simple a maitriser mais n’est pas le plus optimiser en terme de performance. Les autres sont plus efficace mais demandent plus de code pour réaliser en amont pour préparer la scene et les différents paramètres nécessaires 

2d est le plus « simple » pour découvrir les principes d’animations, qui peuvent ensuite être utilisé dans les autres contextes  (webgl, webgl2, gpu, …)

Deal with device pixels : 

La ligne est pixelisée car l’écran a un pixel ratio supérieur a 1, il faut « normaliser » les valeurs en définissant un style width et un style height et en multipliant la taille du canvas par le pixel ratio. Ce principe s’appelle aussi SUPERSAMPLING (SSAA), aka antialiasing pour les joueurs

Les animations sont réalisée avec « requestAnimationFrame » en lui faisant passer une fonction a exécuter à la prochaine frame, avant que le prochain rafraichissement du navigateur soit fait.
L’argument passé dans la fonction est un DOMHighREsTimeStamp, qui est récupérable dans la fonction exécutée et qui permet d’avoir le timestamp depuis la première fois où requestanimationframe a été appelé.
Le timestamp peut ensuite être normalisé en seconde : timestamp /= 1000
Cette valeur de temps va ensuite nous permettre de réaliser nos animations.

### 1-2 - Setting up our app

L'ordre d'ajout fill / stroke a un importance dans le rendu. le dernier passera par desus et peut donc overlapé l'autre.

Il ne faut pas oublier à cleaner le/les parties du canvas que l'on souhaite au début du render pour éviter l'overlaping de rendu dans le canvas, ce qui peut amener de la pixelisation a cause des empilements multiples des rendus. 


## Part 2. Basic trigonometry

### 2-1 - Create a pulsing motion with Math.sin()

Sin: value between [-1, 1]

Le sinus va être utilisé pour les animations

### 2-2 - Circular and elliptical movement

Cos: value between [0, PI]

L'utilisation combinée de Sin et Cos permettent de jouer avec des cercles

### 2-4 - Rotate an object towards a point

ctx.rotate : rotationer le canvas (avec tout les elements à l'intérieur)
ctx.translate : déplacer le canvas (ainsi que son contenu)

ctx.save : a utiliser pour savegarder les matrix et éviter que les autre elements soient affectés par les autres transformations

ctx.restore : pour restaurer l'état précédent du contexte.

const angle = Math.atan2(dy, dx)


## Part 3. Velocity and Acceleration

### 3-1 - Simple velocity on two axes

Les animations doivent être faite basée sur le temps, au lieu d'incrémenter une valeur de façon basique (ex: posX += 0.1), on va plutot calcuyler le temps passé entre deux frames et utiliser cette valeur dans l'animation (const elapsedTime = timestamp - oldTime). On peut ensuite utiliser cette valeur dans nos animations: posX += elapsedTime ... 

### 3-4 - Acceleration and gravity

Différence entre vélocité et accélération : 
- La vélocité décrit la modification de la position d'un objet au cours du temps
- La acceleration est décrit en tant que valeur qui modifie la vélocité au cours du temps

Il n'y a pas d'accélération sans vitesse


## Part 4. Boundaries and friction

Definition Friction:  Résistance au mouvement qui se produit entre deux surfaces en contact.
Ici, la friction (aka damping) est une valeur définie entre 0 et 0 (en général proche de 0.9) permettant de simuler la résistance au mouvement. Ainsi, à chaque colision au sol, la vélocité est réduite en la multipliant par 0.5, tendant sa valeur vers 0 et donc réduire le mouvement du rectangle. La friction en X est également appliquée à chaque render du rectangle, réduisant peu à peu sa vélocité.


## Part 5. Easing and springing


## Part 6. Collision detection

### 6-2 - Distance-based collision detection

Math.sqrt = Renvoie la racine carrée d'un nombre. Utile pour calculer l'hypothenuse 