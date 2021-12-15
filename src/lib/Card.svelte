<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    let rotated = false;
    const rotate = () => {
        if (rotated) return;
        rotated = true;
        dispatch('click', {hodnota, unshow, hide});
    };

    export let hodnota = "";
    let card;
    let hidden = false;

    const unshow = () => rotated = false;
    const hide = () => hidden = true;
</script>

<div class="w-20 h-20 rounded-lg transition-all duration-300 cursor-pointer relative select-none text-white {hidden && "opacity-0"}" style="transform-style: preserve-3d; transform: rotateY({rotated ? "180" : "0"}deg);" on:click={rotate} bind:this={card}>
    <div class="w-full h-full absolute rounded-lg p-2 bg-black" style="backface-visibility: hidden">
        <!-- {hodnota} -->
    </div>
    <div class="w-full h-full absolute rounded-lg bg-red-400 p-2 flex items-center justify-center" style="backface-visibility: hidden; transform: rotateY(180deg);">
        <div class="text-sm">{hodnota}</div>
    </div>
</div>