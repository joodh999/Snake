<script lang="ts">
    import Board from "./components/Board.svelte";
    import { score as s } from "./store";

    let score: number;
    s.subscribe((value) => {
        score = value;
    });
</script>

<main class="m-auto max-w-lg my-10 ">
    <header class="flex justify-between">
        <div class="text-3xl">Snake</div>
        <div>Score: {score}</div>
    </header>
    <Board />
    <div>:)</div>
</main>

<style>
</style>
