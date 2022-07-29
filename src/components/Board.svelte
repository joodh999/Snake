<script lang="ts">
    import { onMount } from "svelte";
    import Cell from "./Cell.svelte";
    import { score } from "../store";

    const initialgrid = () => {
        return Array.from(Array(gridy), () => Array(gridx).fill(0));
    };
    const randomcoords = () => {
        return [
            Math.floor(Math.random() * gridx),
            Math.floor(Math.random() * gridy),
        ];
    };

    const [gridx, gridy] = [20, 20];
    let Grid: number[][] = initialgrid();
    let Snake: { x: number; y: number }[] = [{ x: 5, y: 5 }];
    let Food: number[] = randomcoords();
    let Direction: number[] = [0, 0];

    const Reset = () => {
        Grid = initialgrid();
        Snake = [{ x: 5, y: 5 }];
        Food = randomcoords();
        Direction = [0, 0];
        score.set(0);
    };

    const handleMovement = (e) => {
        switch (e.key) {
            case "w":
            case "ArrowUp":
                if (Direction[1] === 1) break;
                Direction = [0, -1];
                break;
            case "s":
            case "ArrowDown":
                if (Direction[1] === -1) break;
                Direction = [0, 1];
                break;
            case "d":
            case "ArrowRight":
                if (Direction[0] === -1) break;
                Direction = [1, 0];
                break;
            case "a":
            case "ArrowLeft":
                if (Direction[0] === 1) break;
                Direction = [-1, 0];
                break;
            default:
                break;
        }
    };

    const gameOver = () => {
        alert("gameover");
        console.log(Snake.length);
        Reset();
    };

    const moveSnake = () => {
        const newHead = {
            x: Snake[0].x + Direction[0],
            y: Snake[0].y + Direction[1],
        };

        if (
            newHead.x > gridx - 1 ||
            newHead.x < 0 ||
            newHead.y > gridy - 1 ||
            newHead.y < 0
        ) {
            gameOver();
        } else if (newHead.x === Food[0] && newHead.y === Food[1]) {
            Snake = [newHead, ...Snake];
            Food = randomcoords();
            score.update((v) => v + 1);
        } else if (
            Snake.some((segment) => {
                return segment.x === newHead.x && segment.y === newHead.y;
            }) &&
            Snake.length != 1
        ) {
            gameOver();
        } else {
            console.log();
            Snake = [newHead, ...Snake.slice(0, -1)];
        }
    };

    onMount(() => {
        Reset();
        const interval = setInterval(moveSnake, 150);
        return () => clearInterval(interval);
    });

    $: {
        let newGrid = initialgrid();
        Snake.forEach((segment) => {
            newGrid[segment.y][segment.x] = 1;
        });
        newGrid[Food[1]][Food[0]] = 1;
        Grid = newGrid;
    }
</script>

<svelte:window on:keydown={handleMovement} />
<div class="grid grid-cols-20 my-10" on:click={Reset}>
    {#each Grid as row}
        {#each row as cell}
            <Cell value={cell} />
        {/each}
    {/each}
</div>
