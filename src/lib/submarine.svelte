<script lang="ts">
    type coord = { x: number, y: number };
    export let input: coord;
    export let stick: coord = { x: 0, y: 0 };

    let offset = { x: 0, y: 0 } as coord;
    let maxOffset = { x: 800, y: 50 } as coord;

    const interval = setInterval(tick, 10);

    const inputTick = 10;

    function tick () {
        stick.x += ((input.x == 0) ? -Math.sign(stick.x) : Math.sign(input.x)) * inputTick;
        stick.y += ((input.y == 0) ? -Math.sign(stick.y) : Math.sign(input.y)) * inputTick * 0.1;

        stick = zero(stick);
        stick = clamp(stick, maxOffset);

        offset = ease(stick);
    }

    function clamp(input: coord, max: coord): coord {
        return {
            x: Math.sign(input.x) < 0 ? 
                Math.max(max.x * -1, input.x) :
                Math.min(max.x, input.x),
            y: Math.sign(input.y) < 0 ? 
                Math.max(max.y * -1, input.y) :
                Math.min(max.y, input.y),
        };
    }

    function ease(input: coord) {
        return {
            x: Math.log(Math.abs(input.x) + 1) * Math.sign(input.x) * 5,
            y: Math.log(Math.abs(input.y) + 1) * Math.sign(input.y) * 5,
        };
    }

    function zero(input: coord): coord {
        return {
            x: Math.abs(input.x) < 2 ? 0 : input.x,
            y: Math.abs(input.y) < 2 ? 0 : input.y,
        } as coord;
    }
</script>

<div style="transform: translate({offset.x}px, {-offset.y}px)" class="sub"></div>

<style>
    .sub {
        background-color: green;

        width: 40px;
        height: 40px;

        margin: auto;
    }
</style>

