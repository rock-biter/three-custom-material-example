# On BEFORE COMPILE

apri il file main.js per vedere l'esempio

```javascript
const material = new THREE.MeshStandardMaterial({
	color: 'coral',
})
const uniforms = {
	uRedChannel: {
		value: 1,
	},
}

gui.add(uniforms.uRedChannel, 'value', 0, 1, 0.01).name('uRedChannel')
// .onChange((val) => console.log(val))

material.onBeforeCompile = (shader) => {
	shader.uniforms.uRedChannel = uniforms.uRedChannel

	let token = '#include <common>'
	shader.fragmentShader = shader.fragmentShader.replace(
		token,
		/* glsl */ `
		#include <common>
		uniform float uRedChannel;
		`
	)

	token = '#include <color_fragment>'
	shader.fragmentShader = shader.fragmentShader.replace(
		token,
		/* glsl */ `
		#include <color_fragment>

		diffuseColor.r *= uRedChannel;
		`
	)
}

const geometry = new THREE.BoxGeometry(1, 1, 1)
const mesh = new THREE.Mesh(geometry, material)
mesh.position.y += 0.5
scene.add(mesh)
```
