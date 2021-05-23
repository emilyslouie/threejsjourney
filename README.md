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

1. **ArrayCamera** - used to render things like split screens
2. **StereoCamera** - used to render things like VR
3. **CubeCamera** - used to render environment maps for reflection or shadow map
4. **OrthographicCamera** - renders scene without perspective
5. **PerspectiveCamera** - similar to real-life camera with perspective

The PerspectiveCamera has 4 different parameters:

1. **Field of view** - the vertical amplitude angle in degrees; small angle creates long scope effect while wide angle is similar to fish eye effect; typical values are 45-75
2. **Aspect ratio** - can typically use the canvas width and height which should be saved in an object since you use it multiple times
3. **Near (3) and far (4)** - how close and how far the camera can see (range); try not to use too close and too far otherwise you'll get z-fighting where objects render into each other; typically use 0.1 and 100

The OrthographicCamera is different from the PerspectiveCamera and has 6 different parameters: `left`, `right`, `top`, `bottom`, `near`, and `far`. The first four indicate how far the camera can see in those directions.

### Controls

There are some pre-made controls:

- **DeviceOrientationControls** - used to retrieve device orientation if rotation is allowed
- **FlyControls** - lets you rotate all 3 axes, go forward, and go backwards
- **FirstPersonControls** - fixed up axis like a flying bird view where the bird can't do a barrel roll
- **PointerLockControls** - hides the cursor and keeps it centered, allowing you make FPS games
- **OrbitControls** - left click = rotate, right click = panning, mosue wheel = zooming
- **TrackballControls** - similar to OrbitControls but has no limit
- **TransformControls** - doesn't touch the camera, attaches to object and helps to move the object
- **DragControls** - doesn't touch the camera, moves objects on a plane facing the camera by dragging and dropping them

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
