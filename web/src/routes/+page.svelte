<script>
    // @ts-nocheck
    import FluidBackground from "$lib/components/FluidBackground.svelte";
    import Slider from "$lib/components/Slider.svelte";

    let panelWidth = $state(75);
    let dragging = $state(false);
    let mainPanel;
    let animated = $state(true);
    let mainPanelClosed = $state(false);

    let VISCOSITY = $state(0.92);
    let PRESSURE_SPREAD = $state(0.3);
    let PEN_RADIUS = $state(20);

    let fluidApi = $state(null);
    let stampInterval = null;

    async function onready(api) {
        fluidApi = api;
    }

    let frozen = $state(false);

    function showHeadshot() {
        fluidApi?.loadStamp('/headshot.png');
        startStamp();
    }

    function startStamp() {
        frozen = true;
        stampInterval = setInterval(() => {
            fluidApi?.stamp(2, 2, 60, 90, 40);
        }, 50);
    }

    function stopStamp() {
        clearInterval(stampInterval);
        stampInterval = null;
        frozen = false;
    }

    function onMouseDown(e) {
        dragging = true;
        animated = false;
        e.preventDefault();
        mainPanelClosed = false;
    }

    function onMouseMove(e) {
        if (!dragging) return;
        const vw = window.innerWidth;
        const newWidth = ((vw - e.clientX) / vw) * 100;
        panelWidth = Math.min(Math.max(newWidth, 4.5), 75);
    }

    function onMouseUp() {
        dragging = false;
        animated = true;
        if (panelWidth < 30) {
            panelWidth = 4.5;
            mainPanelClosed = true;
        }
        if (panelWidth > 60) {
            panelWidth = 75;
            mainPanelClosed = false;
        }
    }

    function homeBtnPressed() {
        panelWidth = 75;
        mainPanelClosed = false;
        scrollToSection(0);
    }

    const sections = ['Welina', 'Projects', 'Research', 'Blogs'];

    let scrollContainer = $state(null);
    let sectionEls = $state([]);
    let sectionProgresses = $state(sections.map(() => 0));
    let activeSection = $state(0);

    function scrollToSection(index) {
        if (!scrollContainer || !sectionEls[index]) return;
        sectionEls[index].scrollIntoView({ behavior: 'smooth', block: 'start' });
    }

    function onScroll() {
        if (!scrollContainer) return;
        const { scrollTop, scrollHeight, clientHeight } = scrollContainer;

        let newProgresses = sections.map((_, i) => {
            const el = sectionEls[i];
            if (!el) return 0;

            const sectionStart = el.offsetTop;
            const nextEl = sectionEls[i + 1];
            const sectionEnd = nextEl ? nextEl.offsetTop : scrollHeight;
            const sectionHeight = sectionEnd - sectionStart;

            // Scrollable range within this section
            const scrollable = sectionHeight - clientHeight;

            if (scrollable <= 0) {
                // Section is shorter than the viewport — complete once reached
                return scrollTop >= sectionStart ? 100 : 0;
            }

            const scrolledIn = scrollTop - sectionStart;
            if (scrolledIn <= 0) return 0;
            if (scrolledIn >= scrollable) return 100;
            return Math.round((scrolledIn / scrollable) * 100);
        });

        sectionProgresses = newProgresses;

        for (let i = sectionEls.length - 1; i >= 0; i--) {
            if (sectionEls[i] && scrollTop >= sectionEls[i].offsetTop - 4) {
                activeSection = i;
                break;
            }
        }
    }
</script>

<svelte:window onmousemove={onMouseMove} onmouseup={onMouseUp} />

<div class="fixed inset-0 -z-10 bg-[#1b2636]">
    <FluidBackground {PEN_RADIUS} {VISCOSITY} {PRESSURE_SPREAD} {onready} {frozen} />
</div>

<div
    class="fixed z-10 flex justify-between"
    style="
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        padding: 20px;
    "
>
    <!-- Left info panel -->
    <div class="w-[25%] h-full flex items-end pr-12 select-none relative translate-x-1">
        <div class="{mainPanelClosed ? 'opacity-0 z-20' : 'z-30'} absolute transition-all duration-300 w-full h-52 flex flex-col">
            <div class="w-full h-2/3 text-[40px] text-amber-50 font-serif font-[400px]">
                <h1 class="[text-shadow:0px_4px_6px_rgba(0,0,0,0.9)]">
                    Vann Hāwanaloaokekai
                    <br>
                    Siphers
                </h1>
            </div>
            <div class="flex flex-wrap flex-1 justify-start gap-1 mt-3">
                {#each [
                    { label: 'hello@vann-s.com', href: 'mailto:hello@vann-s.com', icon: `<path stroke-linecap="round" stroke-linejoin="round" d="M21.75 6.75v10.5a2.25 2.25 0 0 1-2.25 2.25h-15a2.25 2.25 0 0 1-2.25-2.25V6.75m19.5 0A2.25 2.25 0 0 0 19.5 4.5h-15a2.25 2.25 0 0 0-2.25 2.25m19.5 0v.243a2.25 2.25 0 0 1-1.07 1.916l-7.5 4.615a2.25 2.25 0 0 1-2.36 0L3.32 8.91a2.25 2.25 0 0 1-1.07-1.916V6.75" />` },
                    { label: '(808)866-8458', href: 'tel:+18088668458', icon: `<path stroke-linecap="round" stroke-linejoin="round" d="M2.25 6.75c0 8.284 6.716 15 15 15h2.25a2.25 2.25 0 0 0 2.25-2.25v-1.372c0-.516-.351-.966-.852-1.091l-4.423-1.106c-.44-.11-.902.055-1.173.417l-.97 1.293c-.282.376-.769.542-1.21.38a12.035 12.035 0 0 1-7.143-7.143c-.162-.441.004-.928.38-1.21l1.293-.97c.363-.271.527-.734.417-1.173L6.963 3.102a1.125 1.125 0 0 0-1.091-.852H4.5A2.25 2.25 0 0 0 2.25 6.75Z" />` },
                    { label: 'github', href: 'https://github.com/Van-x2', icon: `<path d="M9 19c-4.3 1.4 -4.3 -2.5 -6 -3m12 5v-3.5c0 -1 .1 -1.4 -.5 -2c2.8 -.3 5.5 -1.4 5.5 -6a4.6 4.6 0 0 0 -1.3 -3.2a4.2 4.2 0 0 0 -.1 -3.2s-1.1 -.3 -3.5 1.3a12.3 12.3 0 0 0 -6.2 0c-2.4 -1.6 -3.5 -1.3 -3.5 -1.3a4.2 4.2 0 0 0 -.1 3.2a4.6 4.6 0 0 0 -1.3 3.2c0 4.6 2.7 5.7 5.5 6c-.6 .6 -.6 1.2 -.5 2v3.5" />` },
                    { label: 'linkedin', href: 'https://www.linkedin.com/in/vann-s/', icon: `<path d="M8 11v5" /><path d="M8 8v.01" /><path d="M12 16v-5" /><path d="M16 16v-3a2 2 0 1 0 -4 0" /><path d="M3 7a4 4 0 0 1 4 -4h10a4 4 0 0 1 4 4v10a4 4 0 0 1 -4 4h-10a4 4 0 0 1 -4 -4l0 -10" />` },
                ] as contact}
                    <a href={contact.href} class="text-sm font-noticia text-amber-50 [text-shadow:0_2px_4px_rgba(0,0,0,0.9)] px-3 py-2 rounded-[6px] transition-all justify-center items-center duration-500 hover:bg-[#121924]/95 hover:backdrop-blur-[1px] flex gap-1">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" class="size-[18px] stroke-amber-50 mr-1 translate-[0px]">
                            {@html contact.icon}
                        </svg>
                        {contact.label}
                    </a>
                {/each}
            </div>
        </div>

        <div class="{mainPanelClosed ? 'z-30' : 'opacity-0 z-20'} absolute flex flex-col justify-end z-20 transition-all duration-300 w-full h-48">
            <div class="w-full flex flex-col">
                <h1 class="text-[18px] text-white font-noticia">Radius</h1>
                <div class="w-full h-8"><Slider bind:value={PEN_RADIUS} min={6} max={80} /></div>
            </div>
            <div class="w-full flex flex-col">
                <h1 class="text-[18px] text-white font-noticia">Viscosity</h1>
                <div class="w-full h-8"><Slider bind:value={VISCOSITY} min={0.6} max={0.990} /></div>
            </div>
            <div class="w-full flex flex-col">
                <h1 class="text-[18px] text-white font-noticia">Pressure Spread</h1>
                <div class="w-full h-8"><Slider bind:value={PRESSURE_SPREAD} min={0.05} max={0.88} /></div>
            </div>
        </div>
    </div>

    <!-- Main panel -->
    <div
        bind:this={mainPanel}
        style="width: {panelWidth}%"
        class="{animated ? 'transition-all duration-300' : ''} h-full backdrop-blur-[1px] bg-[#121924]/95 rounded-2xl drop-shadow-2xl border border-slate-700 p-6 flex pointer-events-auto relative"
    >
        <!-- Drag handle -->
        <div class="w-10 h-36 absolute top-1/2 -translate-y-1/2 translate-x-2 -left-12 bg-[#121924]/95 rounded-bl-2xl rounded-tl-2xl border border-slate-700 border-r-0">
            <div class="w-full h-full justify-center items-center flex flex-col">
                <button aria-label="Resize panel" onmousedown={onMouseDown} class="w-full h-full justify-center items-center flex flex-col cursor-ew-resize">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-8 stroke-white/50 rotate-90 scale-x-400 pointer-events-none">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 9h16.5m-16.5 6.75h16.5" />
                    </svg>
                </button>
            </div>
        </div>

        <!-- Sidebar -->
        <div class="w-10 h-full mr-6 flex shrink-0">

            <!-- Nav buttons -->
            <div class="w-8 flex flex-col">
                <div class="w-8 h-20 mb-6 shrink-0">
                    <div class="w-full h-1/2 flex justify-center items-center">
                        <!-- svelte-ignore a11y_consider_explicit_label -->
                        <button class="cursor-pointer fill-amber-50" onclick={homeBtnPressed}>
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-7 stroke-amber-50">
                                <path stroke-linecap="round" stroke-linejoin="round" d="m2.25 12 8.954-8.955c.44-.439 1.152-.439 1.591 0L21.75 12M4.5 9.75v10.125c0 .621.504 1.125 1.125 1.125H9.75v-4.875c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125V21h4.125c.621 0 1.125-.504 1.125-1.125V9.75M8.25 21h8.25" />
                            </svg>
                        </button>
                    </div>
                    <div class="w-full h-1/2 flex justify-center items-center">
                        <!-- svelte-ignore a11y_consider_explicit_label -->
                        <button class="cursor-pointer fill-amber-50" onclick={() => console.log("Back Button Clicked")}>
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-7 stroke-amber-50">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M9 15 3 9m0 0 6-6M3 9h12a6 6 0 0 1 0 12h-3" />
                            </svg>
                        </button>
                    </div>
                </div>

                <!-- Section nav buttons -->
                <div class="w-8 flex-1 flex flex-col -translate-x-1.5">
                    {#each sections as section, i}
                        <div class="flex-1 flex justify-center items-center">
                            <button
                                onclick={() => scrollToSection(i)}
                                class="cursor-pointer rotate-270 font-noticia text-2xl transition-all duration-300 -translate-x-1 whitespace-nowrap {activeSection === i ? 'text-white' : 'text-amber-50'}"
                            >
                                <h1>{section}</h1>
                            </button>
                        </div>
                    {/each}
                </div>
            </div>

            <!-- 4 stacked progress bars -->
            <div class="w-[5px] flex flex-col ml-1" style="gap: 20px; padding-top: 104px;">
                {#each sections as _section, i}
                    <div class="flex-1 rounded-full bg-slate-700/40 relative overflow-hidden">
                        <div
                            class="absolute top-0 left-0 w-full rounded-full transition-all duration-150"
                            style="
                                height: {sectionProgresses[i]}%;
                                background-color: {activeSection === i
                                    ? 'rgba(255,255,255,0.75)'
                                    : sectionProgresses[i] === 100
                                        ? 'rgba(255,255,255,0.75)'
                                        : 'rgba(255,255,255,0.1)'};
                            "
                        ></div>
                    </div>
                {/each}
            </div>
        </div>

        <!-- Scrollable content -->
        <div
            bind:this={scrollContainer}
            onscroll={onScroll}
            class="flex-1 h-full overflow-y-auto overflow-x-hidden scroll-smooth rounded-xl"
            style="scrollbar-width: none;"
        >
            <!-- Welina -->
            <div
                bind:this={sectionEls[0]}
                class="flex flex-col justify-start pt-6 pl-2"
                style="min-height: 100%;"
            >
                <div class="pb-4 flex items-baseline justify-between">
                    <h2 class="font-serif text-4xl text-amber-50">Welina</h2>
                </div>
                <div class=" bg-gradient-to-r from-slate-700/60 from-0% via-slate-700/60 via-10% to-transparent to-75% h-px mb-8">

                </div>

                <div class="flex flex-col gap-4 text-white font-noticia text-base ml-8">
                    <div class="h-[80vh] flex">
                        <div class="w-[40vw] h-full">
                        <div class="w-full -mb-8">
                            <h1 class=" font-serif text-[100px] leading-35">
                                Welina Mai
                                <br>
                                Kākou,
                            </h1>
                        </div>
                        <div class="w-full">
                            <p class=" font-serif font- text-amber-50/40 text-[38px] text-right leading-13">
                                I'm <button class="underline decoration-white decoration-3 decoration-dotted underline-offset-4 hover:text-white duration-200 cursor-text" onmouseenter={showHeadshot}
                                onmouseleave={stopStamp}>Vann</button>, a Native
                                <br>
                                Hawaiian soul-surfer
                                <br>
                                with a passion for fluid dynamics, studying
                                <br>
                                physics at the University of Hawaii at Mānoa.
                            </p>
                        </div>

                        <div class="w-full h-18 flex justify-center items-center pointer-none:">
                            <img class="mx-8" src="/Waveicon.svg" alt="Wave Icon" />
                            <img class="mx-8" src="/Waveicon.svg" alt="Wave Icon" />
                            <img class="mx-8" src="/Waveicon.svg" alt="Wave Icon" />
                        </div>

                        <div class="w-full">
                            <p class=" font-serif font- text-amber-50/40 text-[38px] text-left text-nowrap leading-13">
                            This site is meant to contain and showcase my 
                            <br>
                            thoughts and experiences that matter to me.
                            <br>
                            Please contact me
                            <br>
                            for any inquiries.
                            </p>
                        </div>

                        </div>
                    </div>
                </div>

                <div class="h-[20vh] w-full ">

                </div>
            </div>

            <!-- Projects -->
            <div
                bind:this={sectionEls[1]}
                class="flex flex-col justify-start pt-6 pl-2"
                style="min-height: 100%;"
            >
                <div class="mb-8 pb-4 border-b border-slate-700/60 flex items-baseline justify-between">
                    <h2 class="font-serif text-4xl text-amber-50">Projects</h2>
                    <span class="font-noticia text-xs text-amber-50/40 uppercase tracking-widest">02 / 04</span>
                </div>

                <div class="flex flex-col gap-4 text-amber-50/60 font-noticia text-base">
                    <p class="leading-relaxed">Projects here</p>
                    <div class="h-[60vh] rounded-xl border border-slate-700/40 flex items-center justify-center text-slate-600 text-sm">
                        [ Projects content ]
                    </div>
                </div>
            </div>

            <!-- Research -->
            <div
                bind:this={sectionEls[2]}
                class="flex flex-col justify-start pt-6 pl-2"
                style="min-height: 100%;"
            >
                <div class="mb-8 pb-4 border-b border-slate-700/60 flex items-baseline justify-between">
                    <h2 class="font-serif text-4xl text-amber-50">Research</h2>
                    <span class="font-noticia text-xs text-amber-50/40 uppercase tracking-widest">03 / 04</span>
                </div>

                <div class="flex flex-col gap-4 text-amber-50/60 font-noticia text-base">
                    <p class="leading-relaxed">Papers, experiments, explorations</p>
                    <div class="h-[60vh] rounded-xl border border-slate-700/40 flex items-center justify-center text-slate-600 text-sm">
                        [ Research content ]
                    </div>
                </div>
            </div>

            <!-- Blogs -->
            <div
                bind:this={sectionEls[3]}
                class="flex flex-col justify-start pt-6 pl-2"
                style="min-height: 100%;"
            >
                <div class="mb-8 pb-4 border-b border-slate-700/60 flex items-baseline justify-between">
                    <h2 class="font-serif text-4xl text-amber-50">Blogs</h2>
                    <span class="font-noticia text-xs text-amber-50/40 uppercase tracking-widest">04 / 04</span>
                </div>

                <div class="flex flex-col gap-4 text-amber-50/60 font-noticia text-base">
                    <p class="leading-relaxed">essays, or thoughts on topics</p>
                    <div class="h-[60vh] rounded-xl border border-slate-700/40 flex items-center justify-center text-slate-600 text-sm">
                        [ Blog content ]
                    </div>
                    <div class="h-[60vh] rounded-xl border border-slate-700/40 flex items-center justify-center text-slate-600 text-sm">
                        [ Blog content ]
                    </div>
                    <div class="h-[60vh] rounded-xl border border-slate-700/40 flex items-center justify-center text-slate-600 text-sm">
                        [ Blog content ]
                    </div>
                    <div class="h-[60vh] rounded-xl border border-slate-700/40 flex items-center justify-center text-slate-600 text-sm">
                        [ Blog content ]
                    </div>
                    <div class="h-[60vh] rounded-xl border border-slate-700/40 flex items-center justify-center text-slate-600 text-sm">
                        [ Blog content ]
                    </div>
                </div>
            </div>
        </div>

    </div>
</div>