<script>
  import "../app.css";
  function closeWindow(){
    window.open("", "_self");
    window.close();
  }
</script>

<div class=" bg-zinc-300 w-full h-[90px] shadow-lg flex">

<div class="h-full w-auto flex ">
  <button on:click={closeWindow()}>
    <div class="w-[50px] h-[50px] bg-zinc-200 rounded-full m-5 content-center shadow-lg">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="4" stroke="currentColor" class="size-6 stroke-amber-500 mx-[13px]">
          <path stroke-linecap="round" stroke-linejoin="round" d="M5.636 5.636a9 9 0 1 0 12.728 0M12 3v9" />
        </svg>   
    </div>
  </button>

  <div class="bg-zinc-200 rounded-full w-[180px] h-[50px] content-center mx-5 my-5 p-2 flex shadow-lg"> 
    <div class=" bg-gradient-to-r from-orange-400 to-rose-600 rounded-full w-16 h-[90%] text-center content-center text-white font-bold">
      
    </div>
    <div class=" w-16 h-[90%] mx-4 text-zinc-500 content-center">
    <span class=" text-[15px] text-nowrap font-bold text-center">
      About Me
    </span>
    </div>
  </div>

  <div class="bg-zinc-200 rounded-full w-[180px] h-[50px] content-center mx-5 mr-12 my-5 p-2 flex shadow-lg"> 
    <div class=" bg-gradient-to-r from-orange-400 to-rose-600 rounded-full w-16 h-[90%] text-center content-center text-white font-bold">
      
    </div>
    <div class=" w-16 h-[90%] mx-3 text-zinc-500 content-center">
    <span class=" text-[15px] text-nowrap font-bold text-center">
      My Projects
    </span>
    </div>
  </div>

</div>


  <div class=" bg-zinc-300 h-full w-[500px] flex-grow content-center">
    <div class="flex justify-center">

      <div class=" w-[33px] h-[70px] border-[3px] rounded-lg border-zinc-200 content-center ml-8 mr-3 bg-zinc-400 shadow-lg">
        <div class="flex">
          <div class=" w-[8px] h-[55px] bg-gray-600 ml-[5px] rounded-sm">
          </div>
            <div class="content-center">
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
            </div>
        </div>
      </div>

      <div class=" w-[33px] h-[70px] border-[3px] rounded-lg border-zinc-200 content-center ml-8 mr-3 bg-zinc-400 shadow-lg">
        <div class="flex">
          <div class=" w-[8px] h-[55px] bg-gray-600 ml-[5px] rounded-sm">
          </div>
            <div class="content-center">
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
            </div>
        </div>
      </div>

      <div class=" w-[33px] h-[70px] border-[3px] rounded-lg border-zinc-200 content-center ml-8 mr-3 bg-zinc-400 shadow-lg">
        <div class="flex">
          <div class=" w-[8px] h-[55px] bg-gray-600 ml-[5px] rounded-sm">
          </div>
            <div class="content-center">
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
            </div>
        </div>
      </div>

      <div class=" w-[33px] h-[70px] border-[3px] rounded-lg border-zinc-200 content-center ml-8 mr-3 bg-zinc-400 shadow-lg">
        <div class="flex">
          <div class=" w-[8px] h-[55px] bg-gray-600 ml-[5px] rounded-sm">
          </div>
            <div class="content-center">
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
              <div class="w-[4px] h-[15px] bg-gray-500 ml-[2px] rounded-sm my-2"></div>
            </div>
        </div>
      </div>

    </div>


    

  </div>

  <div class="  h-full w-[250px] flex-none flex shadow-inner">

    <div class="w-full h-full flex">
      <p class=" text-zinc-800 font-restore ml-12 text-[40px] content-center">
        VANN S
      </p>
    </div>

  </div>
</div>

<slot />