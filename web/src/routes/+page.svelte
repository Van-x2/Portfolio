<script>
  //imports & defines inital variables
  import me1 from '../media/images/portraits/me1.png'
  import { onMount } from 'svelte'
  let canHandleEvent = true
  let currentSlide = 3

  //Function to switch slide
  function switchPage(num, importSlides) {
      let slides = importSlides

      //Iterates through 'slides' list defined above
      for (const slide of slides) {

        //checks if the current slide doesnt have the 'hidden' class for TailwindCSS
        if(!(slide.classList.contains('hidden'))) {
          //adds the hidden tag to the current slide
          slide.classList.toggle('hidden')
        }
      }

      //At this point, all slides have the 'hidden' tag

      //finally, toggles 'hidden' tag for the requested slide, making it visible again
      (eval('slide' + num)).classList.toggle('hidden')
    }

  //Function that runs whenever the 'wheel' listener detects the scroll wheel being usued
  function handleWheel(event) {
      let slides = [slide1, slide2, slide3, slide4]

      //checks if the canHandleEvent variable is set to true
      if (canHandleEvent) {
        //if so, detect the scroll direction

        //if scroll distance is negative AKA going up
        if (event.deltaY < 0) {

          //checks if the currentSlide is anything other than 1 (the first slide)
          if (!(currentSlide == 1)) {
            //if so, increment the currentSlide var by -1 (going to the previous slide)
            currentSlide = currentSlide - 1
          }
        }

        //if scroll distance is positive AKA going down
        if (event.deltaY > 0) {

          //checks if currentSlide var is anything other than the last slide
          if(!(currentSlide == slides.length)) {
            //if so, increment the currentSlide var by +1 (going to the next slide)
            currentSlide = currentSlide + 1
          }
        }

        //sets canHandleEvent to false so that currentSlide cannot be changed until canHandleEvent is reset
        canHandleEvent = false
      }

      //waits 1 sec before changing canHandlelEvent back to true (so that we are not overloaoded with slide changes)
      setTimeout(() => {canHandleEvent = true}, 1000)

      //finally, calls switchPage to switch to the currentSlide
      switchPage(currentSlide, slides)
    }

  function handleKeyDown(event) {
    let slides = [slide1, slide2, slide3, slide4]

    //gets the pressed key
    let key = event.key

    //checks if the key pressed was the up arrow key
    if(key == 'ArrowUp') {
      
      if (!(currentSlide == 1)) {
        //if so, increment the currentSlide var by -1 (going to the previous slide)
        currentSlide = currentSlide - 1
      }
    }

    //checks if the key pressed was the down arrow key
    if(key == 'ArrowDown') {

      if (!(currentSlide == slides.length)) {
        //if so, increment the currentSlide var by +1 (going to the previous slide)
        currentSlide = currentSlide + 1
      }
    }
    switchPage(currentSlide, slides)
  }
  
  //Runs once the DOM is fully loaded
  onMount(() => {

    //imports the slides from the doc into JS object to be used in the script
    const slide1 = document.getElementById('slide1')
    const slide2 = document.getElementById('slide2')
    const slide3 = document.getElementById('slide3')
    const slide4 = document.getElementById('slide4')

    let slides = [slide1, slide2, slide3, slide4]

    //Defines scroll wheel listener
    window.addEventListener('wheel', handleWheel);

    //Defines arrow key listener
    window.addEventListener('keydown', handleKeyDown)
    switchPage(currentSlide, slides)
  })
 

</script>

<div id="webpage" class="w-full h-screen">

  <div id="navbar" class="z-10 relative">
      <div class=" bg-zinc-300 w-full h-[90px] shadow-lg flex border-b-[5px] border-zinc-500">
        
        <div id="nav-group-1" class="h-full w-auto flex">
    
            <div class="w-[50px] h-[50px] bg-zinc-200 rounded-full m-5 shadow-lg flex">
                <button class="content-center">
                  <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="4" stroke="currentColor" class="size-6 stroke-sky-500 mx-[13px]">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M5.636 5.636a9 9 0 1 0 12.728 0M12 3v9" />
                  </svg>
                </button>
       
            </div>
    
            <div class="bg-zinc-200 rounded-full w-[180px] h-[50px] content-center mx-5 my-5 p-2 flex shadow-lg">
    
              <a href="/about" class="flex content-center">
                <div class=" bg-gradient-to-r from-sky-500 to-teal-300 rounded-full w-16 h-[90%] text-center content-center text-white font-bold">
                </div>
        
                <div class=" w-16 h-[90%] mx-4 text-zinc-500 content-center">
                  <span class=" text-[15px] text-nowrap font-bold text-center">
                    About Me
                  </span>
                </div>
              </a>
            </div>
    
            <div class="bg-zinc-200 rounded-full w-[180px] h-[50px] content-center mx-5 mr-12 my-5 p-2 flex shadow-lg"> 
    
              <a href="/projects" class="flex content-center">
              <div class=" bg-gradient-to-r from-sky-500 to-teal-300 rounded-full w-16 h-[90%] text-center content-center text-white font-bold">
              </div>
    
              <div class=" w-16 h-[90%] mx-3 text-zinc-500 content-center">
              <span class=" text-[15px] text-nowrap font-bold text-center">
                My Projects
              </span>
              </div>
              </a>
            </div>
      
        </div>
        
          <div id="nav-group-2" class=" bg-zinc-300 h-full w-[500px] flex-grow content-center">
            <div class="flex justify-end">
    
              <div class="group">
    
                <button class="cursor-default">
                  <div class=" w-[33px] h-[70px] border-[3px] rounded-lg border-zinc-200 content-center ml-4 mr-8 bg-zinc-400 shadow-lg cursor-pointer">
                  <a href="https://www.linkedin.com/in/vann-siphers/" target="_blank" class=" cursor-pointer">
                    <div class="flex">
                      <div class=" w-[8px] h-[55px] bg-gray-600 ml-[5px] rounded-sm">
                      </div>
                        <div class="content-center">
                          <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
                          <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
                        </div>
                    </div>
                  </a>
                  </div>
                </button>
    
                <div class="bg-zinc-300 w-[40px] h-[40px] absolute translate-x-[12px] translate-y-[20px] rounded-lg opacity-0 invisible:delay-300 transition-opacity duration-300 group-hover:opacity-100 group-hover:visible content-center">
                  
                  <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" class=" fill-zinc-600 translate-x-[7px]" width="25" height="25" version="1.1" id="Layer_1" viewBox="0 0 310 310" xml:space="preserve">
                    <g id="XMLID_801_">
                      <path id="XMLID_802_" d="M72.16,99.73H9.927c-2.762,0-5,2.239-5,5v199.928c0,2.762,2.238,5,5,5H72.16c2.762,0,5-2.238,5-5V104.73   C77.16,101.969,74.922,99.73,72.16,99.73z"/>
                      <path id="XMLID_803_" d="M41.066,0.341C18.422,0.341,0,18.743,0,41.362C0,63.991,18.422,82.4,41.066,82.4   c22.626,0,41.033-18.41,41.033-41.038C82.1,18.743,63.692,0.341,41.066,0.341z"/>
                      <path id="XMLID_804_" d="M230.454,94.761c-24.995,0-43.472,10.745-54.679,22.954V104.73c0-2.761-2.238-5-5-5h-59.599   c-2.762,0-5,2.239-5,5v199.928c0,2.762,2.238,5,5,5h62.097c2.762,0,5-2.238,5-5v-98.918c0-33.333,9.054-46.319,32.29-46.319   c25.306,0,27.317,20.818,27.317,48.034v97.204c0,2.762,2.238,5,5,5H305c2.762,0,5-2.238,5-5V194.995   C310,145.43,300.549,94.761,230.454,94.761z"/>
                    </g>
                    </svg>
                  
                </div>
    
              </div>
    
              <div class="group">
    
                <button class="cursor-default">
                  <div class=" w-[33px] h-[70px] border-[3px] rounded-lg border-zinc-200 content-center ml-4 mr-8 bg-zinc-400 shadow-lg cursor-pointer">
                  <a href="https://github.com/Van-x2" target="_blank" class=" cursor-pointer">
                    <div class="flex">
                      <div class=" w-[8px] h-[55px] bg-gray-600 ml-[5px] rounded-sm">
                      </div>
                        <div class="content-center">
                          <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
                          <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
                        </div>
                    </div>
                  </a>
                  </div>
                </button>
    
                <div class="bg-zinc-300 w-[40px] h-[40px] absolute translate-x-[12px] translate-y-[20px] rounded-lg opacity-0 invisible:delay-300 transition-opacity duration-300 group-hover:opacity-100 group-hover:visible">
                  
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1792 1792" class="  fill-zinc-600">
                    <path d="M1664 896q0 251-146.5 451.5t-378.5 277.5q-27 5-39.5-7t-12.5-30v-211q0-97-52-142 57-6 102.5-18t94-39 81-66.5 53-105 20.5-150.5q0-121-79-206 37-91-8-204-28-9-81 11t-92 44l-38 24q-93-26-192-26t-192 26q-16-11-42.5-27t-83.5-38.5-86-13.5q-44 113-7 204-79 85-79 206 0 85 20.5 150t52.5 105 80.5 67 94 39 102.5 18q-40 36-49 103-21 10-45 15t-57 5-65.5-21.5-55.5-62.5q-19-32-48.5-52t-49.5-24l-20-3q-21 0-29 4.5t-5 11.5 9 14 13 12l7 5q22 10 43.5 38t31.5 51l10 23q13 38 44 61.5t67 30 69.5 7 55.5-3.5l23-4q0 38 .5 89t.5 54q0 18-13 30t-40 7q-232-77-378.5-277.5t-146.5-451.5q0-209 103-385.5t279.5-279.5 385.5-103 385.5 103 279.5 279.5 103 385.5z"/>
                  </svg>
                  
    
    
    
                </div>
    
              </div>
    
            </div>
          </div>
        
          <div id="nav-group-3" class="  h-full w-[250px] flex-none flex">
            <div class="w-full h-full flex">
              
                <p class=" text-zinc-800 font-restore ml-12 text-[40px] content-center">
                  <a href="/" class="flex">
                  VANN S
                  </a>
                </p>
    
            </div>
          </div>
    
      </div>
  </div>

  <div id="screen" class="top-[90px] w-full h-full bg-zinc-950 text-white">

      <div id="overlay" class="screen-overlay"></div>

      <div id="screen-content" class="h-screen flex items-center justify-center">

        <div id="slide1" class="absolute hidden w-[75%] h-[50%] text-center translate-y-[-200px] leading-[250px]">
          <h1 class="text-[250px] font-restore ml-[10px] bg-gradient-to-br from-blue-700 via-sky-500 to-green-500 bg-clip-text text-transparent">Vann</h1>
          <p class="text-[250px] font-restore ml-[10px] text-slate-400">x</p>
          <p class="text-[150px] font-restore ml-[10px] text-slate-400">DEVELOPMENT</p>
          <div id="downarrow" class="flex justify-center  ">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class=" animate-bounce duration-200 size-20 content-center text-emerald-500">
              <path stroke-linecap="round" stroke-linejoin="round" d="m4.5 5.25 7.5 7.5 7.5-7.5m-15 6 7.5 7.5 7.5-7.5" />
            </svg>
          </div>

          
        </div>

        <div id="slide2" class="absolute hidden w-[55%] h-[50%] text-center translate-y-[-120px] flex rounded-lg">
          <div class=" w-[40%] h-full flex items-center">
            <div>
              <img src="{me1}" alt="portrait of me, Vann" class="rounded-[40px]">
            </div>
          </div>
          <div class=" w-[60%] h-full text-left flex items-center">
            <div class="m-[50px] mt-[80px]">
                <h1 class=" text-[70px] font-bold"> Aloha!</h1>
              <p class=" text-[20px]">
                Im a 16 year old student from Hawaii,
                <br>
                in 2023 I stumbled into the world of web development and I havent looked back since.
                <br>
                When Im not surfing or hanging around with friends, 
                im coming up with interesting problems to solve with creative website designs.
                <br>
                blah blah blah, I ramble on...
              </p>
            </div>
          </div>
        </div>

        <div id="slide3" class="absolute border-2 border-pink-400 w-[50%] h-[65%] translate-y-[-70px]">
          <div class="w-full h-[25%]  text-center">
            <p class="p-8 text-[70px]">My Technical Pallete</p>
          </div>
          <div class="w-full h-[75%]">
            <div class="w-full h-[33%] bg-sky-300"></div>
            <div class="w-full h-[33%] bg-sky-400"></div>
            <div class="w-full h-[33%] bg-sky-500"></div>
          </div>  
        </div>

        <div id="slide4" class="absolute hidden">
          Slide4
        </div>
        
      </div>

  </div>

</div>

<style lang="postcss">

.screen-overlay {
  background-image:url("https://upload.wikimedia.org/wikipedia/commons/7/76/1k_Dissolve_Noise_Texture.png");
  width: 300%;
  height: 300%;
  opacity: 0.1;
  position: fixed;
  animation: noiseMove 8s steps(10) infinite;
  z-index: 9;
  pointer-events: none;
}

@keyframes noiseMove {
  0%, 100% { transform:translate(0, 0) }
  10%{
    transform:translate(-5%,-10%)
  }
  20%{
    transform:translate(-15%,-20%)
  }
  30%{
    transform:translate(-5%,-10%)
  }
  40%{
    transform:translate(-15%,-20%)
  }
  
  50%{
    transform:translate(-5%,-10%)
  }
  60%{
    transform:translate(-15%,-20%)
  }
  70%{
    transform:translate(-5%,-10%)
  }
  80%{
    transform:translate(-15%,-20%)
  }
  90%{
    transform:translate(-5%,-10%)
  }
  100%{
    transform:translate(-15%,-20%)
  }
}

@keyframes rotate {
from {
  rotate: 0;
}
to {
  rotate: 180;
}
}
</style>