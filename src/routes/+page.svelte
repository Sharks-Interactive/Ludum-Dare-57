<script lang="ts">
    import { onMount } from "svelte";
    import Fish from "$lib/fish.svelte";
    import Submarine from "$lib/submarine.svelte";

    let depth = 0;
    let event: any;

    let input = { x: 0, y: 0 };

    let fish = new Array();

    onMount(() => {
        let interval = setInterval(tick, 10);

        window.addEventListener('touchend', reset);
        window.addEventListener('mouseup', reset);
        window.addEventListener('click', ev => {
            input.x = (ev.pageX - (window.screenX / 2))^0;
            input.y = (ev.pageY - (window.screenY / 2))^0;
        });

        window.addEventListener('deviceorientation', handleOrientation);
        function handleOrientation(ev: any) {
            event = ev;
        }

        for (let i = 0; i < 5000; i++) {
            fish.push({
                x: `${(Math.random() * 800) * (Math.floor((Math.random() * 2) - 1) == 0 ? -1 : 1)}px`,
                y: `${(Math.random() * 5000).toFixed(2)}px`,
            });

            fish = fish;
        }
        console.log(fish);
    });

    function tick() {
        depth -= 0.75;
    }

    function reset() {
        input = { x: 0, y: 0 };
    }
</script>

<div class="strip" style="top: 25px; z-index: 5;">
    {depth.toFixed(2)}
    {event?.alpha?.toFixed(2)} {event?.beta?.toFixed(2)} {event?.gamma?.toFixed(2)}
</div>

<div style:--depth={depth.toFixed(2)} class="water">
    <div style="background-color: #90e4f5; height: 50px;"></div>

    <div style="background-color: rgb(0, 255, 255); height: 10px; width: 100%;"></div>

    {#each fish as pos}
        <Fish {pos}></Fish>
    {/each}
</div>

<div class="strip">
    <Submarine></Submarine>
</div>

<style>
    div {
        max-width: 425px;
        height: 10000vh;

        margin: auto;
    }

    .water {
        transform: translateY(calc(var(--depth) * 1px));
        background-color: rgb(0, calc(255 + (var(--depth) * 0.025)), calc(255 + (var(--depth) * 0.005)));

        overflow: hidden;
    }

    .strip {
        position: fixed;
        top: 50px;
        left: 0;
        right: 0;

        width: 100%;
        height: 40px;
    }
</style>

