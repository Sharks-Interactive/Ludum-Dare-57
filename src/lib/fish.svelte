<script lang="ts">
    export let pos: { x: string, y: string };

    function touching(e1: HTMLElement, e2: HTMLElement) {
        const r1 = e1.getBoundingClientRect();
        const r2 = e2.getBoundingClientRect();

        return !(
            r1.top > r2.bottom ||
            r1.right < r2.left ||
            r1.bottom < r2.top ||
            r1.left > r2.right
        );
    }
</script>

<div class="fish" style="transform: translate({pos.x}, {pos.y})"></div>

<style>
    .fish {
        width: 10px;
        height: 10px;

        background-color: red; 
    }
</style>

