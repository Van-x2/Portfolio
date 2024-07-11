<script>
    //Media Imports
    import mePhoto1 from '../media/images/mePhoto1.png';
    import mePhoto2 from '../media/images/mePhoto2.jpg';
    import KSMparking from '../media/images/KSMparking.jpg';
    import PDFconvert from '../media/images/PDFconvert.jpg';

    //Threlte Imports
    import { Canvas } from '@threlte/core'
    import SceneLeft from './SceneLeft.svelte'
    import SceneRight from './SceneRight.svelte';

    //Svelte Imports
    import { onMount } from 'svelte';

    //Store Import
    import {finalLoaded} from '../stores/loading.js'


    //Defines usefull variables

    //Used in portrait switching function
    let currentMePhoto = 1;
    
    //Used for element manipulation
    let alertText;
    let holoTextLeft
    let photoElements = [];
    let screenElements = [];
    let buttonOverlays = [];

    //Used to take the local place of the store
    let finalLoadedLocal
  
    //Controlls the opacity of the text used with the controller
    function remoteHover() {
    holoTextRight.classList.remove('opacity-0')
    holoTextRight.classList.add('opacity-100')
    }
    function remoteUnHover() {
    holoTextRight.classList.remove('opacity-100')
    holoTextRight.classList.add('opacity-0')
    }
    
    //Controlls the opacity of the text used with the surfboard
    function boardHover() {
    holoTextLeft.classList.remove('opacity-0')
    holoTextLeft.classList.add('opacity-100')
    }
    function boardUnHover() {
      holoTextLeft.classList.add('opacity-0')
      holoTextLeft.classList.remove('opacity-100')
    }

    //Copies my email and toggles the opacity of the notification to show it, then hide it after 2 sec
    function emailPressed() {
      navigator.clipboard.writeText('vannxdevelopment@gmail.com');
      alertText.textContent = 'Email copied to clipboard';
      alertText.classList.add('opacity-100');
      setTimeout(() => { alertText.classList.remove('opacity-100'); }, 2000);
    }
    //Copies my phone and toggles the opacity of the notification to show it, then hide it after 2 sec
    function phonePressed() {
      alertText.classList.remove('opacity-100');
      alertText.classList.add('opacity-0');
      navigator.clipboard.writeText('(808) 866-8458');
      alertText.textContent = 'Phone # copied to clipboard';
      alertText.classList.add('opacity-100');
      setTimeout(() => { alertText.classList.remove('opacity-100'); }, 2000);
    }
    
    //toggles opacity of notification in skills page
    function techStackHovered() {
      alertText.classList.remove('opacity-100');
      alertText.classList.add('opacity-0');
      alertText.textContent = 'My current tech stack';
      alertText.classList.add('opacity-100');
    }
    function techStackUnHovered() {
      alertText.classList.remove('opacity-100');
    }
    function otherHovered() {
      alertText.classList.remove('opacity-100');
      alertText.classList.add('opacity-0');
      alertText.textContent = 'Some other tech that I use';
      alertText.classList.add('opacity-100');
    }
    function otherUnHovered() {
      alertText.classList.remove('opacity-100');
    }
    
    //Switches photo to given (num)
    function alternateMePhotos(num) {
      for (const photo of photoElements) {
        photo.classList.remove('opacity-100');
      }
      photoElements[num - 1].classList.add('opacity-100');
    }
    
    //Iterates slowly and calls alternateMePhotos to continuesly cycle photos
    function cycleMePhotos() {
      if (currentMePhoto > 4) {
        currentMePhoto = 1;
      }
      alternateMePhotos(currentMePhoto);
      currentMePhoto = currentMePhoto + 1;
      setTimeout(() => { cycleMePhotos(); }, 3000);
    }
    
    //Switches content pages to given number
    function switchPages(num) {
      screen1.classList.add('opacity-0')
      for (const screen of screenElements) {
        screen.classList.remove('opacity-100');
        screen.classList.add('pointer-events-none');
      }
      for (const button of buttonOverlays) {
        if(button.classList.contains('opacity-100')) {
          button.classList.remove('opacity-100')
        }
      }
      screenElements[num - 1].classList.add('opacity-100');
      screenElements[num - 1].classList.remove('pointer-events-none');
      buttonOverlays[num -1].classList.add('opacity-100')
    }
    
    //After loading website, sets those variables we defined earlier to different elements from the webpage
    onMount(() => {
      alertText = document.getElementById('alertText')
      holoTextLeft = document.getElementById('holoTextLeft')
      photoElements = [
        document.getElementById('photo1'),
        document.getElementById('photo2'),
        document.getElementById('photo3'),
        document.getElementById('photo4'),
      ];
      screenElements = [
        document.getElementById('screen1'),
        document.getElementById('screen2'),
        document.getElementById('screen3'),
        document.getElementById('screen4'),
      ];
      buttonOverlays = [
        document.getElementById('buttonOverlay1'),
        document.getElementById('buttonOverlay2'),
        document.getElementById('buttonOverlay3'),
        document.getElementById('buttonOverlay4'),
      ];
      //cycleMePhotos();
      document.addEventListener
    });
  </script>
  


  <div id="content-container" class="w-screen h-screen bg-slate-900 text-white flex text-[20px] font-gemeli font-thin">
  
    <div id="left-group" class="h-full w-[27%] ">
      <div class="w-full h-[10%]  top-0 p-8 ">
          <button id="button1" on:click={() => switchPages(1)}>
            About Me
          </button>
          <div id="buttonOverlay1" class="opacity-0 opacity-100 absolute border-white border-r-2 border-b-2 pr-6 pb-4 w-28 h-10 translate-y-[-30px] pointer-events-none duration-200">
          </div>
      </div>
  
      <div class="w-full h-[80%]">
        <div class="absolute h-[80%] w-[27%]">
          <Canvas>
            <SceneLeft on:boardHover={boardHover} on:boardUnHover={boardUnHover}/>
          </Canvas>
        </div>
        <div class=" absolute h-[80%] pointer-events-none">
          <div class="w-full h-[75%] flex items-center translate-x-6">
            <p id="holoTextLeft" class="opacity-0 transition duration-500 italic">
              Along with my passion for programming,
              <br>
              I love spending my free time surfing.
            </p>
          </div>
          <div class="w-full h-[25%]">
  
          </div>
        </div>
      </div>
  
      <div class="w-[27%] h-[10%]  bottom-0 fixed p-8">
        <button id="button3" on:click={() => switchPages(3)}>
          Experience
        </button>
        <div id="buttonOverlay3" class="opacity-0 absolute border-white border-r-2 border-t-2 pr-6 pb-4 w-[135px] h-10 translate-y-[-39px] pointer-events-none duration-200">
        </div>
      </div>
    </div>
  
    <div id="center-group" class="h-full w-[46%] ">
  
      <div id="gradient-border" class="h-[92%] w-[46%] absolute z-10 pointer-events-none hidden">
  
        <div class="w-[60px] h-full left-0 absolute bg-gradient-to-r from-slate-900 to-transparent z-10">
        </div>
  
        <div class="w-[60px] h-full right-0 absolute bg-gradient-to-r from-transparent to-slate-900">
        </div>
  
        <div class="w-full h-[60px] bottom-0 absolute bg-gradient-to-b from-transparent to-slate-900 translate-y-[-35px]">
        </div>
  
  
  
      </div>
  
      <div id="screen-container" class="w-full h-[88%]">
  
        <div id="screen1" class="opacity-0 opacity-100 h-[88%] w-[46%] flex justify-center items-center absolute duration-200">
          <div class=" w-[61%] h-[90%] translate-y-20">
  
            <div class="w-full h-1/2">
              <div class="w-full h-full flex justify-center items-center ">
                <div class="size-80 rounded-full border-[5px] border-white translate-y-4 flex justify-center items-center">
  
                  <img id="photo1" src={mePhoto1} 
                  class="opacity-100 size-[91%] rounded-full absolute duration-500" alt="Vann holding his dog"
                  >
                  
                  <img id="photo2" src={mePhoto2} 
                  class="opacity-0 size-[280px] rounded-full absolute duration-500" alt="Vann holding his dog"
                  >
  
                  <img id="photo3" src={mePhoto1} 
                  class="opacity-0 size-[280px] rounded-full absolute duration-500 rotate-180" alt="Vann holding his dog"
                  >
  
                  <img id="photo4" src={mePhoto2} 
                  class="opacity-0 size-[280px] rounded-full absolute duration-500 rotate-180" alt="Vann holding his dog"
                  >
  
  
                </div>
              </div>
            </div>
  
            <div class="w-full h-1/2 ">
              <p class="text-center p-6 text-[18px]">
                Aloha! Im Vann, a student currently living in Hawaii.
                In 2023 I stumbled upon the world of web development, 
                and I havent looked back since. I have a passion for solving interesting problems with creative solutions.
                <br> 
                (usually in the form of websites)
              </p>
            </div>
  
          </div>
  
        </div>
  
        <div id="screen2" class="pointer-events-none opacity-0 h-[88%] w-[46%] flex justify-center items-center absolute duration-200">
          <div class="w-full h-full ">
  
            <div class="w-full h-[15%]"></div>
  
            <div class="w-full h-[85%] ">
  
              <div class="w-full h-[30%] flex items-end justify-center">
                <div class="w-[80%] h-[80%]  text-center pt-10 px-8 ">
                  <p>
                    I acquire new skills and expertise wherever my newest projects take me,
                    however these are the tools I am most familar with.
                    <br>
                  </p>
                  <p>
                    <br>
                    
                  </p>
                </div>
              </div>
  
              <div class="w-full h-[70%] overflow-hidden">
  
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <div class="mb-12 w-full h-auto py-6 rounded-[50px] border-transparent hover:border-slate-600 border-2 transition duration-300" on:mouseenter={techStackHovered} on:mouseleave={techStackUnHovered}>
                  <div class="w-full h-[50%]  flex items-center justify-center px-4 flex-wrap">
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      HTML
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      CSS
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      JS
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Sveltekit
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Vite
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      TailwindCSS
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Pocketbase
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Express
                    </div>
  
                  </div>
                </div>
  
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <div class=" w-full h-auto py-6 rounded-[50px] border-transparent hover:border-slate-600 border-2 transition duration-300" on:mouseenter={otherHovered} on:mouseleave={otherUnHovered}>
                  <div class="w-full h-[50%]  flex items-center justify-center px-4 flex-wrap">
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Docker
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Figma
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Git
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Github
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Adobe
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      More
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      To
                    </div>
  
                    <div class="m-2 text-[20px] w-[150px] px-6  h-14 border-[1px] pointer-events-none bg-slate-900 rounded-full flex justify-center items-center">
                      Come!
                    </div>
  
                  </div>
                </div>
  
              </div>
  
            </div>
  
          </div>
        </div>
  
        <div id="screen3" class=" pointer-events-none opacity-0 h-[88%] w-[46%] flex justify-center items-center absolute duration-200">
          <div class="w-full h-full">
  
            <div class="w-full h-[15%]"></div>
  
            <div class="w-full h-[85%] ">
              <div class="w-full h-[100%] overflow-x-hidden overflow-y-auto ">
  
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <div class=" w-full h-auto py-6 rounded-[50px] border-transparent border-2 transition duration-300">
                  <div class="w-full h-[50%]  flex items-center justify-center px-4 flex-wrap">
  
                    <div class="m-2 text-[16px] w-[80%] p-2  h-24 border-[1px] pointer-events-none bg-slate-900 rounded-[20px]">
                      Internship
                      <br>
                      Tech Partners Hawaii
                      <br>
                      2022 -> present
                    </div>
  
                  </div>
                </div>
  
              </div>
  
            </div>
  
          </div>
        </div>
  
        <div id="screen4" class=" pointer-events-none opacity-0 h-[88%] w-[46%] flex justify-center items-center absolute duration-200">
          <div class="w-[85%] h-[90%] grid gap-x-[20px] gap-y-[20px] justify-center translate-y-14" style="grid-template-columns: 380px 380px; grid-template-rows: 150px 150px 150px 150px">
  
            <div class="border-2 rounded-[20px] h-[150px] overflow-hidden relative group" >
                <div class="w-full h-full flex relative">
                  <div class=" opacity-90 w-[48%] h-[100%] bg-slate-700 group-hover:w-[100%] duration-200">
                    <div class="w-full h-[67%]" >
                      <div class="pl-2 w-full h-[40%] p-[5px] text-nowrap overflow-hidden text-ellipsis font-bold text-[20px]">
                        <span>KSM Parking Website</span>
                      </div>
                      <div class="w-full h-[60%]  p-[5px] overflow-hidden text-ellipsis text-[17px] opacity-80">
                        <span>A website to let students reserve parking spots at my high school.</span>
                      </div>
                    </div>
                    <div class="w-full h-[33%] flex items-center pl-2" >
                      <div class="w-[37px] h-[37px] bg-white rounded-full mx-2 flex items-center justify-center">
                        <a href="https://ksmparking.fly.dev/" target="_blank">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="size-8 fill-slate-900 ">
                            <path fill-rule="evenodd" d="M8.914 6.025a.75.75 0 0 1 1.06 0 3.5 3.5 0 0 1 0 4.95l-2 2a3.5 3.5 0 0 1-5.396-4.402.75.75 0 0 1 1.251.827 2 2 0 0 0 3.085 2.514l2-2a2 2 0 0 0 0-2.828.75.75 0 0 1 0-1.06Z" clip-rule="evenodd" />
                            <path fill-rule="evenodd" d="M7.086 9.975a.75.75 0 0 1-1.06 0 3.5 3.5 0 0 1 0-4.95l2-2a3.5 3.5 0 0 1 5.396 4.402.75.75 0 0 1-1.251-.827 2 2 0 0 0-3.085-2.514l-2 2a2 2 0 0 0 0 2.828.75.75 0 0 1 0 1.06Z" clip-rule="evenodd" />
                          </svg>
                        </a>
                      </div>
  
                      <div class="w-[37px] h-[37px] bg-white rounded-full mx-2 flex items-center justify-center">
                        <a href="https://github.com/Van-x2/Parking-Project" target="_blank">
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1792 1792" class="size-8 fill-slate-900">
                            <path d="M1664 896q0 251-146.5 451.5t-378.5 277.5q-27 5-39.5-7t-12.5-30v-211q0-97-52-142 57-6 102.5-18t94-39 81-66.5 53-105 20.5-150.5q0-121-79-206 37-91-8-204-28-9-81 11t-92 44l-38 24q-93-26-192-26t-192 26q-16-11-42.5-27t-83.5-38.5-86-13.5q-44 113-7 204-79 85-79 206 0 85 20.5 150t52.5 105 80.5 67 94 39 102.5 18q-40 36-49 103-21 10-45 15t-57 5-65.5-21.5-55.5-62.5q-19-32-48.5-52t-49.5-24l-20-3q-21 0-29 4.5t-5 11.5 9 14 13 12l7 5q22 10 43.5 38t31.5 51l10 23q13 38 44 61.5t67 30 69.5 7 55.5-3.5l23-4q0 38 .5 89t.5 54q0 18-13 30t-40 7q-232-77-378.5-277.5t-146.5-451.5q0-209 103-385.5t279.5-279.5 385.5-103 385.5 103 279.5 279.5 103 385.5z"/>
                          </svg>
                        </a>
                      </div>
  
  
                    </div>
                  </div>
                  <div class=" opacity-90 w-[20%] h-[100%] bg-gradient-to-r from-slate-700 to-transparent group-hover:w-[0%] duration-200"></div>
                  <div class="w-[30%] h-[100%] group-hover:w-[0%] duration-200"></div>
                  <div class="w-full h-full absolute overflow-hidden -z-10">
                    <img src={KSMparking} alt="A website that I made to let kids reserve parking spots at my highschool">
                  </div>
                </div>
              
            </div>
  
            <div class="border-2 rounded-[20px] h-[150px] overflow-hidden relative group" >
              <div class="w-full h-full flex relative">
                <div class=" opacity-90 w-[48%] h-[100%] bg-slate-700 group-hover:w-[100%] duration-200">
                  <div class="w-full h-[67%]" >
                    <div class="pl-2 w-full h-[40%] p-[5px] text-nowrap overflow-hidden text-ellipsis font-bold text-[20px]">
                      <span>Image to PDF Website</span>
                    </div>
                    <div class="w-full h-[60%]  p-[5px] overflow-hidden text-ellipsis text-[17px] opacity-80">
                      <span>A small website that compiles image files into PDF format.</span>
                    </div>
                  </div>
                  <div class="w-full h-[33%] flex items-center pl-2" >
                    <div class="w-[37px] h-[37px] bg-white rounded-full mx-2 flex items-center justify-center">
                      <a href="https://imagestopdf.fly.dev/" target="_blank">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="size-8 fill-slate-900 ">
                          <path fill-rule="evenodd" d="M8.914 6.025a.75.75 0 0 1 1.06 0 3.5 3.5 0 0 1 0 4.95l-2 2a3.5 3.5 0 0 1-5.396-4.402.75.75 0 0 1 1.251.827 2 2 0 0 0 3.085 2.514l2-2a2 2 0 0 0 0-2.828.75.75 0 0 1 0-1.06Z" clip-rule="evenodd" />
                          <path fill-rule="evenodd" d="M7.086 9.975a.75.75 0 0 1-1.06 0 3.5 3.5 0 0 1 0-4.95l2-2a3.5 3.5 0 0 1 5.396 4.402.75.75 0 0 1-1.251-.827 2 2 0 0 0-3.085-2.514l-2 2a2 2 0 0 0 0 2.828.75.75 0 0 1 0 1.06Z" clip-rule="evenodd" />
                        </svg>
                      </a>
                    </div>
  
                    <div class="w-[37px] h-[37px] bg-white rounded-full mx-2 flex items-center justify-center">
                      <a href="https://github.com/Van-x2/ImageToPDFWebsite" target="_blank">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1792 1792" class="size-8 fill-slate-900">
                          <path d="M1664 896q0 251-146.5 451.5t-378.5 277.5q-27 5-39.5-7t-12.5-30v-211q0-97-52-142 57-6 102.5-18t94-39 81-66.5 53-105 20.5-150.5q0-121-79-206 37-91-8-204-28-9-81 11t-92 44l-38 24q-93-26-192-26t-192 26q-16-11-42.5-27t-83.5-38.5-86-13.5q-44 113-7 204-79 85-79 206 0 85 20.5 150t52.5 105 80.5 67 94 39 102.5 18q-40 36-49 103-21 10-45 15t-57 5-65.5-21.5-55.5-62.5q-19-32-48.5-52t-49.5-24l-20-3q-21 0-29 4.5t-5 11.5 9 14 13 12l7 5q22 10 43.5 38t31.5 51l10 23q13 38 44 61.5t67 30 69.5 7 55.5-3.5l23-4q0 38 .5 89t.5 54q0 18-13 30t-40 7q-232-77-378.5-277.5t-146.5-451.5q0-209 103-385.5t279.5-279.5 385.5-103 385.5 103 279.5 279.5 103 385.5z"/>
                        </svg>
                      </a>
                    </div>
  
  
                  </div>
                </div>
                <div class=" opacity-90 w-[20%] h-[100%] bg-gradient-to-r from-slate-700 to-transparent group-hover:w-[0%] duration-200"></div>
                <div class="w-[30%] h-[100%] group-hover:w-[0%] duration-200"></div>
                <div class="w-full h-full absolute overflow-hidden -z-10">
                  <img src={PDFconvert} alt="A website that I made to let kids reserve parking spots at my highschool">
                </div>
              </div>
            
          </div>
  
          </div>
        </div>
  
      </div>
  
      <div id="contact-container" class="bottom-0 w-[46%] h-[12%] fixed ">
        <div id="text-container" class=" w-[100%] h-[50%] text-center text-[15px]">
          <div id="text-wrapper" class=" translate-y-6">
            <span id="alertText" class=" opacity-0 duration-500 text-slate-600"> 
              Hi! this is just some testing text.
            </span>
          </div>
        </div>
        <div id="icon-container" class="flex justify-center h-[6%] w-[46%] bottom-0 fixed">
          <div class="border-white border-b-[4px] w-fit p-4 h-full flex justify-center items-center ">
  
            <div id="linkdin-icon" class="w-12 h-12 mx-2 flex items-center justify-center hover:-translate-y-[5px] duration-100">
              <a href="https://www.linkedin.com/in/vann-siphers/" target="_blank">
                <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" class=" fill-white w-6 h-6 hover:fill-slate-400 duration-100" version="1.1" id="Layer_1" viewBox="0 0 310 310" xml:space="preserve">
                  <g id="XMLID_801_">
                    <path id="XMLID_802_" d="M72.16,99.73H9.927c-2.762,0-5,2.239-5,5v199.928c0,2.762,2.238,5,5,5H72.16c2.762,0,5-2.238,5-5V104.73   C77.16,101.969,74.922,99.73,72.16,99.73z"/>
                    <path id="XMLID_803_" d="M41.066,0.341C18.422,0.341,0,18.743,0,41.362C0,63.991,18.422,82.4,41.066,82.4   c22.626,0,41.033-18.41,41.033-41.038C82.1,18.743,63.692,0.341,41.066,0.341z"/>
                    <path id="XMLID_804_" d="M230.454,94.761c-24.995,0-43.472,10.745-54.679,22.954V104.73c0-2.761-2.238-5-5-5h-59.599   c-2.762,0-5,2.239-5,5v199.928c0,2.762,2.238,5,5,5h62.097c2.762,0,5-2.238,5-5v-98.918c0-33.333,9.054-46.319,32.29-46.319   c25.306,0,27.317,20.818,27.317,48.034v97.204c0,2.762,2.238,5,5,5H305c2.762,0,5-2.238,5-5V194.995   C310,145.43,300.549,94.761,230.454,94.761z"/>
                  </g>
                </svg>
              </a>
            </div>
    
            <div id="github-icon" class="w-12 h-12 mx-2 flex items-center justify-center hover:-translate-y-[5px] duration-100">
                <a href="https://github.com/Van-x2" target="_blank">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1792 1792" class=" fill-white w-8 h-8 hover:fill-slate-400 duration-100">
                    <path d="M1664 896q0 251-146.5 451.5t-378.5 277.5q-27 5-39.5-7t-12.5-30v-211q0-97-52-142 57-6 102.5-18t94-39 81-66.5 53-105 20.5-150.5q0-121-79-206 37-91-8-204-28-9-81 11t-92 44l-38 24q-93-26-192-26t-192 26q-16-11-42.5-27t-83.5-38.5-86-13.5q-44 113-7 204-79 85-79 206 0 85 20.5 150t52.5 105 80.5 67 94 39 102.5 18q-40 36-49 103-21 10-45 15t-57 5-65.5-21.5-55.5-62.5q-19-32-48.5-52t-49.5-24l-20-3q-21 0-29 4.5t-5 11.5 9 14 13 12l7 5q22 10 43.5 38t31.5 51l10 23q13 38 44 61.5t67 30 69.5 7 55.5-3.5l23-4q0 38 .5 89t.5 54q0 18-13 30t-40 7q-232-77-378.5-277.5t-146.5-451.5q0-209 103-385.5t279.5-279.5 385.5-103 385.5 103 279.5 279.5 103 385.5z"/>
                  </svg>
                </a>
            </div>
    
    
    
            <div id="email-icon" class="w-12 h-12 mx-2 flex items-center justify-center hover:-translate-y-[5px] duration-100">
              <button on:click={emailPressed}>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="fill-white hover:fill-slate-400 duration-100 size-6">
                  <path d="M1.5 8.67v8.58a3 3 0 0 0 3 3h15a3 3 0 0 0 3-3V8.67l-8.928 5.493a3 3 0 0 1-3.144 0L1.5 8.67Z" />
                  <path d="M22.5 6.908V6.75a3 3 0 0 0-3-3h-15a3 3 0 0 0-3 3v.158l9.714 5.978a1.5 1.5 0 0 0 1.572 0L22.5 6.908Z" />
                </svg> 
              </button>         
            </div>
    
            <div id="phone-icon" class="w-12 h-12 mx-2 flex items-center justify-center hover:-translate-y-[5px] duration-100">
              <button on:click={phonePressed}>
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="fill-white hover:fill-slate-400 duration-100 size-6">
                  <path fill-rule="evenodd" d="M1.5 4.5a3 3 0 0 1 3-3h1.372c.86 0 1.61.586 1.819 1.42l1.105 4.423a1.875 1.875 0 0 1-.694 1.955l-1.293.97c-.135.101-.164.249-.126.352a11.285 11.285 0 0 0 6.697 6.697c.103.038.25.009.352-.126l.97-1.293a1.875 1.875 0 0 1 1.955-.694l4.423 1.105c.834.209 1.42.959 1.42 1.82V19.5a3 3 0 0 1-3 3h-2.25C8.552 22.5 1.5 15.448 1.5 6.75V4.5Z" clip-rule="evenodd" />
                </svg>
              </button>                    
            </div>
    
          </div>
        </div>
      </div>
  
    </div>
  
    <div id="right-group" class="h-full w-[27%] ">
  
      <div class="w-full h-[10%]  top-0 text-right p-8">
        <button id="button2" on:click={() => switchPages(2)}>
          Skills
        </button>
        <div id="buttonOverlay2" class="opacity-0 absolute border-white border-l-2 border-b-2 pr-6 pb-4 w-28 h-10 translate-y-[-30px] pointer-events-none right-[15px] duration-200">
        </div>
      </div>
  
      <div class="w-full h-[80%]">
        <div class="absolute h-[80%] w-[27%] ">
          <Canvas>
            <SceneRight on:remoteHover={remoteHover} on:remoteUnHover={remoteUnHover}/>
          </Canvas>
        </div>
        <div class=" absolute h-[80%] pointer-events-none">
          <div class="w-full h-[50%] flex items-center translate-x-6">
            <p id="holoTextRight" class="opacity-0 transition duration-500 italic -translate-x-12 hover:opacity-100">
              I am also an avid gamer, currently
              <br>
              playing & recommend: 
              <a href="https://store.steampowered.com/app/1809540/Nine_Sols/" target="_blank" class="pointer-events-auto">
                <span class=" bg-white hover:bg-gradient-to-r from-red-700 via-yellow-300 to-green-400 bg-clip-text text-transparent duration-500 transition hover:font-bold">
                  Nine Sols
                </span>
              </a>
            </p>
          </div>
          <div class="w-full h-[50%]">
  
          </div>
        </div>
      </div>
  
      <div class="w-[27%] h-[10%]  bottom-0 fixed text-right p-8">
        <button id="button4" on:click={() => switchPages(4)} >
          Projects
        </button>
        <div id="buttonOverlay4" class="opacity-0 absolute border-white border-l-2 border-t-2 pr-6 pb-4 w-28 h-10 translate-y-[-40px] pointer-events-none right-[32px] duration-200">
        </div>
      </div>
  
    </div>
  
  </div>
  <div id="warnbox" class="hidden absolute w-screen h-screen bg-gray-600 flex items-center justify-center">
    <div class="w-[90%] h-fit rounded-[50px] bg-gray-300">
      <div class="w-full h-1/2 flex justify-center items-center p-8">
        <span class="text-[40px] font-bold text-red-800 text-center">
          Incompatable Screen Resolution Detected
        </span>
      </div>
      <div class="w-full h-1/2 flex justify-center items-center p-8">
        <p class="text-[20px] -translate-y-4 text-center">
          This webpage is designed for desktop web browsers only, please resize the window to continue.
        </p>
      </div>
    </div>
  </div>

  <style lang="postcss">
  @media (max-width: 1350px) {
    #content-container {
      visibility: hidden;
      position: absolute;
    }
    #warnbox {
      display: flex;
    }
  }
  @media (max-height: 750px) {
    #content-container {
      visibility: hidden;
      position: absolute;
    }
    #warnbox {
      display: flex;
    }
  }

  </style>