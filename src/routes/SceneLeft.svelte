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

    //Defines 'surfboard' as the linked glb file
    const surfBoard = useLoader(GLTFLoader).load('https://cdn.tinyglb.com/models/f4cedb874eb449ce9687c5ab27faddc9.glb')

    //Defines other variables
    let boardRotation = spring(0)
    let canBoardRotate = true
    let tempRotation = 0
    let boardPosX = spring(-1000)
    let boardPosY = spring(0)
    let boardRotationTemp = 0
    let canBeHovered = false
    let allLoaded = false

    //useTask is called every frame, with delta (difference between frames)
    useTask((delta)=>{
        //if we allow the board to rotate then change the boards rotation by half of the frame delta, every frame
        if(canBoardRotate) {
        boardRotationTemp =+ (boardRotationTemp + (delta/2))
        boardRotation.set(boardRotationTemp)
        }
        //if we dont allow the board to rotate then set the boards rotation to its default
        if(!canBoardRotate) {
        boardRotationTemp = -1.5
        }
        //Everytime this is called, check if the boards rotation is above 4.76 (a full rotation)
        //if so, reset the boards rotation to its default rotation
        if (boardRotationTemp >= 4.76) {
            boardRotationTemp = -1.5
            boardRotation.update(() => -1.5, { hard: true });
        }
        
    })

    //Functions to handle hovering over the 3D object
    function boardLoad() {
    boardPosX.set(-500)
    }
    function boardHover() {
    boardRotation.set(-1.5)
    boardPosX.set(-400)
    dispatch('boardHover')
    }
    function boardUnHover() {
    boardRotation.set(0)
    boardPosX.set(-500)
    dispatch('boardUnHover')
    }
    

    //this is called every time the surfboard file changes
$: if ($surfBoard) {
    //if surfboard exists then go through every 'child' (basicly component) of the scene of the surfboard.glb file
    $surfBoard.scene.traverse((child) => {
        //for each child check if the child is a mesh
        if (child.isMesh) {
            //if it is a mesh, then set the material to desired state
            child.material = new MeshPhongMaterial({ color: 'white', wireframe: false });
        }
    });
    }

    //every time the finalLoaded store is changed, this function is run
    finalLoaded.subscribe((value) => {
        //if finalLoaded = true, then call the boardLoad() after 500 ms, bringign the file into the scene
        if (value == true) {
            setTimeout(()=>{
                boardLoad()
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
    scale={220}
    position.x={-300}
    position.y={-40}
    visible={false}
    
    on:pointerenter={()=> {
        if(canBeHovered) {
        boardHover()
        canBoardRotate = false
        }
        }}
    on:pointerleave={()=> {
        if(canBeHovered) {
        canBoardRotate = true
        boardUnHover()
        }
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
    scale={60}
    rotation={[$boardRotation, 0, 1.58]}
    position={[$boardPosX, -40, -800]}
  />
{/if}
