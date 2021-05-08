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
