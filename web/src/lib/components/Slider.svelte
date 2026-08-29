<script>
  let { value = $bindable(1), min = 0, max = 100 } = $props();
  let dragging = false;
  let track;

  function getPct(e) {
    const r = track.getBoundingClientRect();
    const x = e.touches ? e.touches[0].clientX : e.clientX;
    return Math.min(1, Math.max(0, (x - r.left) / r.width));
  }

  function setValue(e) { value = min + getPct(e) * (max - min); }

  function onMouseDown(e) { dragging = true; setValue(e); }
  function onMouseMove(e) { if (dragging) setValue(e); }
  function onMouseUp()    { dragging = false; }
  function onTouchStart(e) { dragging = true; setValue(e); }
  function onTouchMove(e)  { if (dragging) setValue(e); }
  function onTouchEnd()    { dragging = false; }

  let pct = $derived(((value - min) / (max - min)) * 100);
</script>

<svelte:window
  on:mousemove={onMouseMove}
  on:mouseup={onMouseUp}
  on:touchmove={onTouchMove}
  on:touchend={onTouchEnd}
/>

<div
  bind:this={track}
  class="relative flex w-full cursor-pointer items-center py-4"
  on:mousedown={onMouseDown}
  on:touchstart={onTouchStart}
  role="slider"
  aria-valuenow={value}
  aria-valuemin={min}
  aria-valuemax={max}
  tabindex="0"
>
  <div class="h-2 w-full rounded-sm bg-[#121924] border border-slate-600"></div>
  <div
    class="absolute h-6 w-3 -translate-x-1/2 bg-[#121924]"
    style="left: {pct}%;"
  ></div>
</div>