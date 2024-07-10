5<script>
    import { T, useTask } from '@threlte/core'
    import { interactivity } from '@threlte/extras'
    import { spring } from 'svelte/motion'
    import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
    import { useLoader } from '@threlte/core'
    import { BoxGeometry, MeshBasicMaterial, EdgesGeometry, LineSegments, LineBasicMaterial, MeshPhongMaterial, DefaultLoadingManager } from 'three'
    import { createEventDispatcher } from 'svelte'
    import { finalLoaded } from '../stores/loading.js'
    const dispatch = createEventDispatcher()

    const surfBoard = useLoader(GLTFLoader).load('https://cdn.tinyglb.com/models/f4cedb874eb449ce9687c5ab27faddc9.glb')


    

    interactivity()

    let boardRotation = spring(0)
    let canBoardRotate = true
    let tempRotation = 0
    let boardPosX = spring(-1000)
    let boardPosY = spring(0)
    let boardRotationTemp = 0

    let canBeHovered = false

    let allLoaded = false


    useTask((delta)=>{

        if(canBoardRotate) {
        boardRotationTemp =+ (boardRotationTemp + (delta/2))
        boardRotation.set(boardRotationTemp)
        }
        if(!canBoardRotate) {
        boardRotationTemp = -1.5
        }
        if (boardRotationTemp >= 4.76) {
            boardRotationTemp = -1.5
            boardRotation.update(() => -1.5, { hard: true });
        }
        
    })

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
    


$: if ($surfBoard) {
    $surfBoard.scene.traverse((child) => {
        if (child.isMesh) {
            child.material = new MeshPhongMaterial({ color: 'white', wireframe: false });
        }
    });
    }


    finalLoaded.subscribe((value) => {
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
