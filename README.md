# Three.js Journey

This is a repository hosting code used throughout my time learning Three.js from [Bruno Simon's Three.js course](https://threejs-journey.xyz). Below are some notes about the basic usages of Three.js.

## Basic parts to a scene

- **Scene** which is essentially the set and made by `const scene = new THREE.Scene();`
- **Objects** that make up the scene; these consist of a geometry (shape), and material
- **Camera** which views the scene and is made up of a perspective, and position
- **Canvas** which was added to the HTML and is rendered in the JS through a **renderer**; the scene and camera are added to the renderer

## Objects (Mesh)

- made up of a geometry and material

Example:

```javascript
const cube1 = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
```

Some methods to manipulate meshes:

- `mesh.position.x/y/z` translates the mesh to different points in space
- `mesh.scale.x/y/z` scales the mesh to be bigger or smaller along an axis
- `mesh.rotation.x/y/z` rotates the mesh along an axis in radians

Objects can be added to a `new THREE.Group()` so that the entire group is manipulated rather than applying the same transformations to each object individually.

## Basic Animation

A simple way to make a Three.js animation is to make an infinite loop function that manipulates the object or camera and then rerenders the scene using `window.requestAnimationFrame()`.

The problem is that different computers have different framerates which makes the speed at which something rerenders inconsistent among computers.

Possible solutions:

- use the `Date` function in JS to regulate the time at which it rerenders
- use the `THREE.Clock()` function

There is also another library called GSAP which can be used to animate objects since it works with Three.js.

## Cameras

There are different types of cameras:

1. `ArrayCamera` - used to render things like split screens
2. `StereoCamera` - used to render things like VR
3. `CubeCamera` - used to render environment maps for reflection or shadow map
4. `OrthographicCamera` - renders scene without perspective
5. `PerspectiveCamera` - similar to real-life camera with perspective

The PerspectiveCamera has 4 different parameters:

1. `Field of view` - the vertical amplitude angle in degrees; small angle creates long scope effect while wide angle is similar to fish eye effect; typical values are 45-75
2. `Aspect ratio` - can typically use the canvas width and height which should be saved in an object since you use it multiple times
3. `Near (3) and far (4)` - how close and how far the camera can see (range); try not to use too close and too far otherwise you'll get z-fighting where objects render into each other; typically use 0.1 and 100

The OrthographicCamera is different from the PerspectiveCamera and has 6 different parameters: `left`, `right`, `top`, `bottom`, `near`, and `far`. The first four indicate how far the camera can see in those directions.

### Controls

There are some pre-made controls:

- `DeviceOrientationControls` - used to retrieve device orientation if rotation is allowed
- `FlyControls` - lets you rotate all 3 axes, go forward, and go backwards
- `FirstPersonControls` - fixed up axis like a flying bird view where the bird can't do a barrel roll
- `PointerLockControls` - hides the cursor and keeps it centered, allowing you make FPS games
- `OrbitControls` - left click = rotate, right click = panning, mosue wheel = zooming
- `TrackballControls` - similar to OrbitControls but has no limit
- `TransformControls` - doesn't touch the camera, attaches to object and helps to move the object
- `DragControls` - doesn't touch the camera, moves objects on a plane facing the camera by dragging and dropping them

#### OrbitControls

- can use `target` to change the postion of where the camera is targeting for x, y, z
  ex. `controls.target.y = 2`
- damping will smooth the animation by adding acceleration and friction which can be enabled by `controls.enableDamping = true`

## Geometries

There is the `BoxGeometry` which has been used up until now, but what is a geometry? Geometries are made up of vertices and faces. These are used to make meshes and can be used to make particles.

There are a lot of built in geometries in Three.js which start with the following and end in `Geometry`:

- `Box` - box
- `Plane` - rectangle plane
- `Circle` - disk or part of a disk
- `Cone` - cone or part of a cone
- `Ring` - flat ring or portion of flat circle
- `Torus` - donut type ring
- `TorusKnow` - knot thing
- `Dodecahedron` - 12 faced sphere
- `Octahedron` - 8 faced sphere
- `Tetrahedron` - 4 faced sphere
- `Icosahedron` - sphere made of triangles with the same size
- `Sphere` - basically a sphere with the quads
- `Shape` - shape based on a path
- `Tube` - tube following a path
- `Extrude` - making a extrusion based on a path
- `Lathe` - creates a vase
- `Text` - creates 3D text

As for the parameters of these built in geometries, there is the `width`, `height`, `depth`, `widthSegments`, `heightSegments`, and `depthSegments`. It works on spheres to change the segment parameters so that it is more smooth. By default it is 1, so if you set it to more, then there will be more segments and appear more smooth.

We can use `BufferGeometry` to make our own shapes. This involves creating an array with the locations of the points, and the number of points used per shape in the `BufferAttribute` since you can make multiple of the same shape using the array and methods.

## Textures

- images that cover the surface of geometries

There are different types of textures:

- **Color** which takes the pixels of the texture and applies it to the geometry
- **Alpha** which takes a grayscale image where while is visible and black is not
- **Height** which is a grayscale image that moves the vertices to create some relief
- **Normal** which will add small details but not move the vertices
- **Ambient occlusion** which is a grayscale image that will fake shadow on surface crevices
- **Metalness** which is a grayscale image that will show metallic parts as white and non-metallic as black helping to create a reflection
- **Roughness** is a grayscale image that goes with metalness and helps to dissipate the light

**PBR** (physically based rendering) principles is one of the regrouping techniques that follow real life situations to get more realistic renderings and results.

- use the `.TextureLoader()` to load the image, it also has progress function for when the loading is happening and an error funnction if something goes wrong

- use `.LoadingManager()` to help you manage loading a number of textures into your project

- use **UV Unwrapping** when we need to wrap a texture around a weird shaped object; if we have to create our own geometry, we have to also do the UV unwrapping ourselves through the 3D software

**Mipmapping** is a technique that consists of creating half a smaller version of a texture again and again until you get a 1x1 texture. There are two filter algorithms: minification and magnification filters. Only use the mipmaps for the minFiler property and you don't need it if you are using `THREE.NearestFilter`, so you can deactivate it using `colorTexture.generateMipmaps = false`.

**Minification filter** is when the pixels of texture are smaller than the pixels of the render, so texture is too big for the surface and has a couple of different filters:

- `THREE.NearestFilter`
- `THREE.LinearFilter`
- `THREE.NearestMipmapNearestFilter`
- `THREE.NearestMipmapLinearFilter`
- `THREE.LinearMipmapNearestFilter`
- `THREE.LinearMipmapLinearFilter`

Using the `THREE.NearestFilter` results in some weird effects happening called moiré patterns which render crazy things.

**Magnification filter** is similar to minification filter but it makes it beeger so that the texture is too small for the surface to cover, and thus is more blurry. This one has two filters:

- `THREE.NearestFilter` - kinda like the Minecraft pixelated effect, gets the best performance out of all the options
- `THREE.LinearFilter`

With textures, need to keep in mind the **weight**, **size**, and **data**. For weight, the images need to be downloaded upon visiting the site, so need to make it as compressed as possible. For size, reduce the pixel size as much as possible and needs to be a power of 2. For data, textures support transparency, and need to use pngs for lossless compression.

Some places to find textures include:

- [poliigon.com](https://www.poliigon.com)
- [3dtextures.me](https://3dtextures.me)
- [arroway-textures.ch](https://www.arroway-textures.ch)

## Materials

- used to put color on each visible picture of a geometry

`MeshBasicMaterial` is the most basic material and using the map property will let you add a texture while the color property lets you add a color class

- one can combine the `color` and `map` properties to tint the texture with a colour
- the `wireframe` property lets you see it as a geometry
- one can also change the opacity of a material, first set `transparent = true` and then set `opacity = #`
- `alphaMap` is a grayscale texture that controls the opacity across the surface (black: fully transparent; white: fully opaque)
- `side` lets you choose which side is visible (either front, back or double sides)

`MeshNormalMaterial` lets you see the rainbowy colours in the normal relative's orientation to the camera; with this you can also use `flatShading` which flattens the faces

- cool portfolio using the normals is [https://www.ilithya.rocks](https://www.ilithya.rocks)

`MeshMatcapMaterial` is an illuminated looking material that is performative
`MeshDepthMaterial` will colour the geometry in white if it is close to the camera and black if it is far

There are some that need to use a light source in order to render properly:

- `MeshLambertMaterial` is a material that renders based on lighting in the scene
- `MeshPhongMaterial` uses the same things as the previous but with less strange patterns and can change `shininess` and `specular` properties
- `MeshToonMaterial` makes it look like it came out of a cartoon and you can use the `gradientMap` property to declare colours for the shadow and for the light in which we need to ensure we use a large enough gradient texture

`MeshStandardMaterial` uses the standard physically based rendering principles and you can change the `metalness` and `roughness` properties

- can use `aoMap` property to add shadows where the textures are dark by giving the UV coordinates
- the `displacementMap` properties will move the vertices to create true relief and need to make sure we have enough displacement and more subdivisions by changing the scale
- `normalMap` will fake the normal orientation and add details on the surface regardless of the subdivision; there is also the `metalnessMap` and `roughnessMap`

An **environment** map is an image of what's happening in the surrounding scene and is used to add reflection or refraction to the objects, makes it similar to a mirror or shiny object. A good resource for environment maps are [HDRIHaven](https://hdrihaven.com) and you need to convert it to a cube map using https://matheowis.github.io/HDRI-to-CubeMap/.

## Fonts

In three.js we can make 3D text. First, we need to make a .json of the font face and then add it to the project. Then, we need to load it in. Afterwards, we can use the same functions on objects to change the materials.

We can use `textGeometry.center()` to centre the text.

A good resource for getting a matcap is https://github.com/nidorx/matcaps.

## Lighting

`AmbientLight` applies omnidirectional lighting on all the geometries of the scene, taking parameters such as `color` and `intensity`.

`DirectionalLight` has a sun effect with sun rays going in parallel.

`HemisphereLight` is similar to `AmbientLight` but has a different colour coming from the sky and from the ground. This makes it so that there are two colour parameters and an intensity one.

`PointLight` is like a lighter where the light is very small and is uniform. You can change the fade distance and how quickly it fades using the `distance` and `decay` properties.

`RectAreaLight` is similar to large rectangle lights and is similar to directional lighting and diffuse light. There are the `color`, `intensity` parameters, but also `width` of the rectangle and `height`.

`SpotLight` is like a flashlight and has the first two parameters and `distance`, `angle`, `penumbra`, and `decay` parameters.

Some performace considerations:
**Minimal cost** - AmbientLight, HemisphereLight

**Moderate cost** - DirectionalLight, PointLight

**High cost** - SpotLight, RectAreaLight

Baking is when you put the light into the texture, but you can't move the lights.

## Shadows

With lights we also ened shadows. The back part of objects that are dark are called **core shadows**. The **drop shadows** are the shadows created on other objects.

When doing a render, Three.js will do a render for all lights that cast shadows. These will simulate the lights seens as if it were a camera. When the light renders, `MeshDepthMaterial` replaces all mesh materials. It's then stored as textures and named shadow maps.

To set it up, you have to enable the `shadowMap`, set up `castShadow` and `receiveShadow` on the objects that have shadows, and then make the light cast the shadow with `castShadow`.

The problem with baking the shadow into a texture is that it is not dynamic.

## Particles

Parts to make a particle: `BufferGeometry`, `PointsMaterial` and `Points`

`PointsMaterial` is a special type of material to create particles. It lets us use `size`, and `sizeAttenuation` to specify if distant particles should be smaller than close particles.

`alphaTest` is a value between 0 and 1 that lets WebGL know when and when not to render pixel based on transparency.

`depthTest` lets you test to see if what's being drawn is closer than what's already drawn. Doesn't work well when there are other objects in the scene though.

A `depth buffer` is the depth of what's being drawn is stored.

`depthWrite` lets us tell WebGL to not write particles in that depth buffer. And works ok in our case with particles and other objects in the scene.

`blending` is a property that lets us not only to draw to a pixel, but to add the color of that pixel to the color of the pixel already drawn. So in our case, it lets us see the halo effect on top of another halo, making it super saturated in the places that they overlap. This effect reduces the performance though, so make sure to use less particles.

## Raycaster

Shoots a ray in a direction to test to see which objects intersect with the ray. The two main methods are `intersectObject` and `intersectObjects`.

Each item of that returned array contains much useful information:

- distance: the distance between the origin of the ray and the collision point.
- face: what face of the geometry was hit by the ray.
- faceIndex: the index of that face.
- object: what object is concerned by the collision.
- point: a Vector3 of the exact position in 3D space of the collision.
- uv: the UV coordinates in that geometry.
