<script>
    import { T, useTask } from '@threlte/core'
    import { interactivity } from '@threlte/extras'
    import { spring } from 'svelte/motion'
    import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
    import { useLoader } from '@threlte/core'
    import { BoxGeometry, MeshBasicMaterial, EdgesGeometry, LineSegments, LineBasicMaterial, MeshPhongMaterial, AmbientLight, Light } from 'three'
    import { createEventDispatcher } from 'svelte'
    const dispatch = createEventDispatcher()

    const surfBoard = useLoader(GLTFLoader).load('https://cdn.tinyglb.com/models/5e9775680d1d4019a39857b3061b913c.glb')

    interactivity()
    let objectMaterial
    let remoteRotation = spring(0)
    let canRemoteRotate = true
    let tempRotation = 0
    let remotePosX = spring(500)
    let remotePosY = spring(0)
    let remoteRotationTemp = 0


    useTask((delta)=>{

        if(canRemoteRotate) {
        remoteRotationTemp =+ (remoteRotationTemp + (delta/2))
        remoteRotation.set(remoteRotationTemp)
        }
        if(!canRemoteRotate) {
        remoteRotationTemp = 0
        }
        if (remoteRotationTemp >= 6.3) {
            remoteRotationTemp = 0
            remoteRotation.update(() => 0, { hard: true });
        }
        
    })

    function remoteLoad() {
    remotePosX.set(230)
    }
    function remoteHover() {
    remoteRotation.set(0)
    remotePosX.set(150)
    dispatch('remoteHover')
    }
    function remoteUnHover() {
    remoteRotation.set(0)
    remotePosX.set(230)
    dispatch('remoteUnHover')
    }
    

    // Wait for the model to load and then set wireframe materials
    $: if ($surfBoard) {
    $surfBoard.scene.traverse((child) => {
        if (child.isMesh) {
            child.material = new MeshPhongMaterial({ color: 'white', wireframe: false});
            objectMaterial = child.material
        }
        
    });
    }

    setTimeout(()=>{remoteLoad()}, 5000)
</script>

<T.OrthographicCamera 
    makeDefault
    lookAt={[0, 0, 0]}
    position.y={0}
/>

<T.Mesh
    position.z={-800}
    scale={230}
    position.x={400}
    position.y={-70}
    visible={false}
    
    on:pointerenter={()=> {
    remoteHover()
    canRemoteRotate = false
        }}
    on:pointerleave={()=> {
    canRemoteRotate = true
    remoteUnHover()
        }}
>
    <T.BoxGeometry args={[4,1.2,1]}/>
    <T.MeshBasicMaterial wireframe={true} wireframeLinewidth={0.5} color="white"
    />
</T.Mesh>

<T.DirectionalLight position={[0, 10, 10]} 
intensity={10}
/>
<T.AmbientLight 
intensity={1}
/>



{#if $surfBoard}
  <T
    is={$surfBoard.scene}
    scale={1200}
    rotation={[$remoteRotation, 3.15, 0]}
    position={[$remotePosX, -60, -800]}
  />
{/if}
