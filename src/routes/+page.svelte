<script lang="ts">
    import Spacer from "$lib/spacer.svelte";
    import { words } from "$lib/words";

    let guessList: string[] = [];
    let word = 'chargement...';

    let guess = '';
    let valid = true;

    let rest = 30;
    let timer = rest;
    let score = 0;

    let active = false;
    let end = false;

    let level = 0;

    let content: HTMLElement;
    let interval: number;

    let size = 100000;

    function start() {
        interval = setInterval(tick, 1000);
        word = words[level].word;

        active = true;
        //window?.visualViewport?.addEventListener("resize", () => {
            //size = window?.visualViewport?.height ?? window.innerHeight;
        //});
    }

    function tick() {
        timer--;

        if (timer <= 0) {
            if (level == 4) {
                clearInterval(interval);
                end = true;
                return;
            }
            
            level++;
            timer = 30;

            score += guessList.length;
            guessList = [];

            word = words[level].word;
        }
    }

    function submit() {
        if (!words[level].synonyms.includes(guess.replaceAll('-', '').replaceAll(' ', '').toLowerCase()) || guessList.includes(guess)) {
            content.classList.remove('shake');
            valid = true;
            valid = false;
            return;
        }
        
        guessList.push(guess);
        guessList = guessList;

        guess = '';
    }

    let phrases = [
        'come up with as many synonyms as you can',
        'what is the deeper meaning?',
        'what is it trying to say?',
        'how does that make you feel?',
        'but what does it *really* mean?',
    ];
</script>

{#if !active}

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; max-width: 425px; margin: auto;">
    <div style="min-height: 30px"></div>
    <h1>meaning.</h1>
    <p>
        You know the irritating feeling when you know what a word means, but can't explain it? <br />
        Get ready to experience that several times over... <br />
    </p>
    <h2>what to expect:</h2>
    <p>
        Once you hit start, you will be given a word and then have {rest} seconds to come up with as many
        synonyms for it as possible - in other words, you have to dig deeper into the true meaning of the word.
        After the {rest} seconds is up, you'll move onto the next word, 5 total.
    </p>
    <div style="min-height: 30px"></div>
    <button style="aspect-ratio: unset; padding: 10px 15px 10px 15px" on:click={start}>start</button>
</div>

{:else if !end}

<div class="strip">
    <Spacer flex={2}></Spacer>

    <h1>{timer}s to ponder:</h1>
    <h2>{word}</h2>
    <Spacer flex={2}></Spacer>

    <p>{phrases[level]}</p>
    <div style="display: flex; flex-direction: row; gap: 15px" bind:this={content} class={valid ? '' : 'shake'}>
        <input 
            type="text" bind:value={guess} on:keyup={ev => ev.key == 'Enter' ? submit() : () => {}}
            style="color: {valid ? 'white' : 'red'}" on:keypress={() => valid = true}
        >
        <button on:click={submit} on:touchstart={() => valid = true}>✔</button>
    </div>
    <Spacer></Spacer>

    <h3 style="flex: 3">
        <span>past words:</span> <br />
        {#each guessList as guess}
            <span>{guess}</span> <br />
        {/each}
    </h3>

    <Spacer flex={5}></Spacer>
</div>

{:else}

<div style="display: flex; flex-direction: column; justify-content: center; align-items: center; max-width: 425px; margin: auto;">
    <div style="min-height: 30px"></div>
    <h1>thanks for playing...</h1>
    <p>
        this was thrown together in a matter of four hours or so, thanks to<br />
        a busy weekend + I've been in a bit of a Ludum Dare rut recently..<br />
    </p>
    <h2>so thank you anyway for playing this 'game'</h2>
    <p>
        Your final score was <strong>{score}</strong> points!
    </p>
    <div style="min-height: 30px"></div>
</div>

{/if}

<style>
    .strip {
        max-width: 425px;
        height: 100vh;

        background-color: #181d22;

        margin: auto;
        padding: 15px;

        display: flex;
        flex-direction: column; 

        justify-content: space-between;
        align-items: center;
    }

    h1, h2, h3 {
        margin: 0;
    }

    h2, h3 {
        font-weight: normal;
    }

    button {
        background-color: #2a303b;
        border-radius: 12px;

        border: none;
        outline: none;

        font-size: 1rem;
        font-weight: bold;

        color: white;

        aspect-ratio: 1 / 1;
        height: 100%;
    }

    input {
        outline: 1px solid #2a303b;
        border: none;

        width: 100%;
        max-width: 250px;

        padding: 10px 20px 10px 20px;
        color: white;

        text-align: center;

        font-size: 1rem;

        border-radius: 12px;
        background-color: #181d22;
    }
    
    .shake {
        animation: shake 0.5s;
    }

    @keyframes shake {
        0% { transform: translateX(1px) }
        10% { transform: translateX(-1px) }
        20% { transform: translateX(-3px) }
        30% { transform: translateX(3px) }
        40% { transform: translateX(1px) }
        50% { transform: translateX(-1px) }
        60% { transform: translateX(-3px) }
        70% { transform: translateX(3px) }
        80% { transform: translateX(-1px) }
        90% { transform: translateX(1px) }
        100% { transform: translateX(1px) }
    }
</style>

