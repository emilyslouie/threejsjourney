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
