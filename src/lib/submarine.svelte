<script lang="ts">
    type coord = { x: number, y: number };
    export let input: coord;
    export let stick: coord = { x: 0, y: 0 };

    let offset = { x: 0, y: 0 } as coord;
    let maxOffset = { x: 10, y: 5 } as coord;

    const interval = setInterval(tick, 10);

    const inputTick = 0.02;

    function tick () {
        stick.x += ((input.x == 0) ? stick.x^0 : input.y^0) * inputTick;
        stick.y += ((input.y == 0) ? stick.y^0 : input.y^0) * inputTick;

        stick = clamp(stick, maxOffset);

        offset = ease(input);
    }

    function clamp(input: coord, max: coord): coord {
        return {
            x: Math.min(max.x, input.x),
            y: Math.min(max.y, input.y),
        };
    }

    function ease(input: coord) {
        return {
            x: input.x^2,
            y: input.y^2,
        };
    }
</script>

<div class="sub"></div>

<style>
    .sub {
        background-color: green;

        width: 40px;
        height: 40px;

        margin: auto;
    }
</style>

