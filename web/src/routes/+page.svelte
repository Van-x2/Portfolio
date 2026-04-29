<script>
// @ts-nocheck

    import FluidBackground from "$lib/components/FluidBackground.svelte";
</script>

<div class="fixed inset-0 -z-10 bg-[#1b2636]">
    <FluidBackground />
</div>

<div class="fixed inset-0 z-10 flex justify-end p-6">

    <div class="w-1/3 h-full flex items-end pr-12">
    </div>

    <div class="w-2/3 h-full backdrop-blur-[1px] bg-[#121924]/95 rounded-2xl drop-shadow-2xl border border-slate-700 p-6 flex pointer-events-auto">
        <div class="w-8 h-full mr-6 flex flex-col">
            <div class="w-full h-20 mb-6">
                <div class="w-full h-1/2 flex justify-center items-center">
                    <!-- svelte-ignore a11y_consider_explicit_label -->
                    <button class=" cursor-pointer" onclick={console.log("Home Button Clicked")}>
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" class="size-7">
                            <path stroke-linecap="round" stroke-linejoin="round" d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
                        </svg>
                    </button>
                </div>
                <div class="w-full h-1/2 flex justify-center items-center">
                    <!-- svelte-ignore a11y_consider_explicit_label -->
                    <button class=" cursor-pointer" onclick={console.log("Back Button Clicked")}>
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="white" class="size-7">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M9 15 3 9m0 0 6-6M3 9h12a6 6 0 0 1 0 12h-3" />
                        </svg>
                    </button>
                </div>
            </div>
            <div class="w-full flex-1 bg-red-500">
            <div class="w-full h-1/4 flex justify-center items-center">
                <button class="cursor-pointer rotate-270 font-no text-2xl">
                    <h1>
                        Philosophy
                    </h1>
                </button>
            </div>
            </div>
        </div>
        <div class="flex-1 h-full">

        </div>
    </div>
</div>