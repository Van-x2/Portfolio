<script>
    //imports from ThreeJS
    import { BoxGeometry, MeshBasicMaterial, EdgesGeometry, LineSegments, LineBasicMaterial, MeshPhongMaterial, DefaultLoadingManager } from 'three'
    import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'

    //imports from Threlte
    import { T, useTask } from '@threlte/core'
    import { interactivity } from '@threlte/extras'
    import { useLoader } from '@threlte/core'

    //imports from svelte
    import { createEventDispatcher } from 'svelte'
    import { spring } from 'svelte/motion'

    //imports from store
    import { finalLoaded } from '../stores/loading.js'

    //calls interactivity, allowing for interaction with 3D objects
    interactivity()

    //Defines 'dispatch' as an event dispatcher
    const dispatch = createEventDispatcher()

    //Defines 'remote' as the linked glb file
    const remote = useLoader(GLTFLoader).load('https://cdn.tinyglb.com/models/5e9775680d1d4019a39857b3061b913c.glb')

    //defines other variables
    let objectMaterial
    let remoteRotation = spring(0)
    let canRemoteRotate = true
    let tempRotation = 0
    let remotePosX = spring(500)
    let remotePosY = spring(0)
    let remoteRotationTemp = 0
    let canBeHovered

    //useTask is called every frame, with delta (difference between frames)
    useTask((delta)=>{
        //if we allow the board to rotate then change the boards rotation by half of the frame delta, every frame
        if(canRemoteRotate) {
        remoteRotationTemp =+ (remoteRotationTemp + (delta/2))
        remoteRotation.set(remoteRotationTemp)
        }
        //if we dont allow the remote to rotate then set the boards rotation to its default
        if(!canRemoteRotate) {
        remoteRotationTemp = 0
        }
        //Everytime this is called, check if the remotes rotation is above 4.76 (a full rotation)
        //if so, reset the remote rotation to its default rotation
        if (remoteRotationTemp >= 6.3) {
            remoteRotationTemp = 0
            remoteRotation.update(() => 0, { hard: true });
        }
        
    })

    //Functions to handle hovering over the 3D object
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
    

    //this is called every time the surfboard file changes
    $: if ($remote) {
    //if remote exists then go through every 'child' (basicly component) of the scene of the remote.glb file
    $remote.scene.traverse((child) => {
        //for each child check if the child is a mesh
        if (child.isMesh) {
            //if it is a mesh, then set the material to desired state
            child.material = new MeshPhongMaterial({ color: 'white', wireframe: false});
            objectMaterial = child.material
        }
        
    });
    }

    //function is called after object is fully loaded
    DefaultLoadingManager.onLoad = function ( ) {
    //sets value of store to true
    finalLoaded.set(true)
};


    //every time the finalLoaded store is changed, this function is run
    finalLoaded.subscribe((value) => {
        //if finalLoaded = true, then call the remoteLoad() after 500 ms, bringign the file into the scene
        if (value == true) {
            setTimeout(()=>{
                remoteLoad()
                canBeHovered = true
            }, 500)
        }
    })



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
    position.y={-20}
    visible={false}
    
    on:pointerenter={()=> {
        if(canBeHovered) {
            remoteHover()
            canRemoteRotate = false
        }
        }}
    on:pointerleave={()=> {
        if(canBeHovered) {
            canRemoteRotate = true
            remoteUnHover()
        }
        }}
>
    <T.BoxGeometry args={[4,1.5,1]}/>
    <T.MeshBasicMaterial wireframe={true} wireframeLinewidth={0.5} color="white"
    />
</T.Mesh>

<T.DirectionalLight position={[0, 10, 10]} 
intensity={10}
/>
<T.AmbientLight 
intensity={1}
/>



{#if $remote}
  <T
    is={$remote.scene}
    scale={1200}
    rotation={[$remoteRotation, 3.15, 0]}
    position={[$remotePosX, -60, -800]}
  />
{/if}
