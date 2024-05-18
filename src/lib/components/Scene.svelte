<script>
  import { T } from '@threlte/core'
  import { Float, OrbitControls } from '@threlte/extras'
  import Mug from './mug.svelte'
  import { SheetObject } from '@threlte/theatre'
  import { useSequence } from '@threlte/theatre'

  export let animationOver

  const { position, config, play, playing } = useSequence()
  config({ iterationCount: 1 })
  let started = false;
  $: {
    if($playing) started = true;
    if(!$playing && started) {
      animationOver();
    }
  }
  play()
</script>


<!-- <Grid
  position.y={-0.001}
  cellColor="#ffffff"
  sectionColor="#ffffff"
  sectionThickness={0}
  fadeDistance={25}
  cellSize={2}
/> -->

<!-- <ContactShadows
  scale={10}
  blur={2}
  far={2.5}
  opacity={0.5}
/> -->


<T.PerspectiveCamera
makeDefault
position={[-11, 0, 5]}
fov={25}
on:create={({ ref }) => {
  ref.lookAt(0, -20, -10)
}}
>

<OrbitControls
  
  enableZoom={false}
  enableDamping
  autoRotateSpeed={0.5}
  target.y={1.1}
  target.x={0.4}
/>
</T.PerspectiveCamera>

<SheetObject
key="Box"
  let:Transform
  let:Sync
>


  <Transform>
    
    <Mug position={[0, 0, 0]} scale={[20, 20, 20]}></Mug>
  </Transform>
  
  <T.DirectionalLight
  color='gold'
  intensity={0.4}

  position={[-12, 0, 5]}
  on:create={({ ref }) => {
    ref.lookAt(0, 0, 0)
  }}
  >
    <Sync intensity='directional' />
  </T.DirectionalLight>
  <T.DirectionalLight
  color='white'
  intensity={0.7}

  position={[-8, 0, 5]}
  on:create={({ ref }) => {
    ref.lookAt(0, 0, 0) 
  }}
  >
    <Sync intensity='lit' />
  </T.DirectionalLight>
  <T.AmbientLight  color={'white '} intensity={1.0}><Sync intensity='ambient' /></T.AmbientLight>
</SheetObject>


<!-- <T.Mesh
  rotation.y={-Math.PI / 2}
  position.x={-10}
  receiveShadow
>
  <T.CircleGeometry args={[4, 40]} />
  <T.MeshStandardMaterial color="white" />
</T.Mesh>

<Float
  floatIntensity={1}
  floatingRange={[0, 1]}
>
  <T.Mesh
    position={[1.2, 1.5, 7.5]}
    rotation.x={5}
    rotation.y={71}
  >
    <T.TorusKnotGeometry args={[0.5, 0.15, 100, 12, 2, 3]} />
    <T.MeshStandardMaterial color="#F85122" />
  </T.Mesh>
</Float>

<Float
  floatIntensity={1}
  floatingRange={[0, 1]}
>
  <T.Mesh
    position={[.4, 1.5, 7.5]}
    rotation={[-5, 128, 10]}
  >
    <T.IcosahedronGeometry />
    <T.MeshStandardMaterial color="#F8EBCE" />
  </T.Mesh>
</Float> 
-->
