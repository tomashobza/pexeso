<script>
    import Card from "$lib/Card.svelte";
    import shuffle from "lodash/shuffle";
    import { fade } from "svelte/transition";

    let content = ['prcat', 'dujku', 'protoze', 'neni', 'zena', 'nema', 'rad', 'jane', 'austenovou', 'xdd'];

    $: console.log(shuffle([...content, ...content]));
    $: cards = shuffle([...content, ...content]);

    let chosen = [];
    let foundPairs = 0;
    let badGuesses = 0;
    let won = false;

    const click = (e) => {
        if (chosen.length >= 2) {
            chosen.forEach((card) => card.unshow());
            chosen = [];
        }

        chosen.push(e.detail);
        
        if (chosen.length == 2) {
            if (chosen.every((val, i, arr) => val.hodnota === arr[0].hodnota)) {
                foundPairs += 1;
                chosen.forEach((card) => card.hide());
                chosen = [];

                if (foundPairs == content.length) {
                    won = true;
                }
            } else {
                badGuesses += 1;
            }
        }
    }
</script>

<div class="flex flex-col justify-center items-center h-full">
    <div class="flex flex-row justify-around w-full text-xl md:w-1/2 font-bold py-4">
        <div class="text-green-400">Párů nalezeno: {foundPairs}</div>
        <div class="text-red-400">Špatných tipů: {badGuesses}</div>
    </div>
    <div class="flex flex-col items-center justify-center flex-grow">
        {#if won}
            <div class="grid grid-cols-4 grid-rows-5 gap-2">
                {#each cards as card}
                    <Card hodnota={card} on:click={click} />
                {/each}
            </div>
        {:else}
            <div class="text-2xl text-gray-300 select-none" in:fade>Gratuluji, vyhrál jsi</div>
        {/if}
    </div>
</div>

