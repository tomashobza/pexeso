<script>
    import Card from "$lib/Card.svelte";
    import shuffle from "lodash/shuffle";
    import { fade } from "svelte/transition";

    let content = ['java', 'c', 'html', 'python', 'css', 'js', 'c#', 'c++', 'ruby', 'perl', 'swift', 'assembler', 'scratch', 'brainfuck', 'rust', 'php', 'kotlin', 'pascal', 'sql', 'go', 'fortran', 'typescript', 'dart', 'solidity'];

    $: contentProcessed = content.slice(0, (difficulty + 2)*4);

    $: cards = shuffle([...contentProcessed, ...contentProcessed]);

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

                if (foundPairs == contentProcessed.length) {
                    won = true;
                }
            } else {
                badGuesses += 1;
            }
        }
    }

    const newGame = () => {
        diffSelected = false;
        won = false;
        foundPairs = 0;
    };

    const obtiznostString = ["easy", "medium", "difficult"];
    let difficulty = 0;
    let diffSelected = false;
</script>

<div class="flex flex-col justify-center items-center h-full">
    <div class="flex flex-row justify-around w-full text-xl md:w-1/2 font-bold py-4">
        <div class="text-green-400">Párů nalezeno: {foundPairs}</div>
        <div class="text-red-400">Špatných tipů: {badGuesses}</div>
    </div>
    <div class="flex items-center justify-center gap-2">
        <label for="obtiznost">Obtížnost:</label>
        <input type="range" id="obtiznost" name="obtiznost" min="0" max="2" step="1" bind:value={difficulty}  disabled={diffSelected}>
        <div class="w-20 text-gray-400">{obtiznostString[difficulty]}</div>
        <button class="text-white font-bold py-2 px-4 text-sm cursor-pointer select-none rounded-full {diffSelected ? "bg-gray-400":"bg-blue-500 hover:bg-blue-700"}" disabled={diffSelected} on:click={() => diffSelected = true}>Uložit</button>
        <button class="text-white font-bold py-2 px-4 text-sm cursor-pointer select-none rounded-full bg-red-500 hover:bg-red-700" on:click={newGame}>Nová hra</button>
    </div>
    <div class="flex flex-col items-center justify-center flex-grow overflow-auto">
        {#if diffSelected}
            {#if !won}
                <div class="grid grid-cols-{2 * (difficulty + 2)} gap-2">
                    {#each cards as card}
                        <Card hodnota={card} on:click={click} />
                    {/each}
                </div>
            {:else}
                <div class="text-2xl text-yellow-500 select-none" in:fade>Gratuluji, vyhrál jsi!</div>
            {/if}
        {:else}
            <div class="text-2xl text-gray-300 select-none" in:fade>Pro hraní, prosím zvolte otížnost...</div>
        {/if}
    </div>
</div>

