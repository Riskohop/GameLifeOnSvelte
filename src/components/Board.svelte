<script>
    import { onMount } from "svelte";

    export let height;
    export let width;
    export let boxSize;
    export let stateColor;
    let canv;
    let boardCells;
    onMount(() => {
        boardCells = Array.from({ length:boxSize }, () => (
            Array.from({ length:boxSize }, () => {return {
                state: Math.random() >= 0.5 ? "died" : "live"
            }})
        ));
        canv = document.querySelector('#boardCanvas');
        updateBoard(true);

    });
    function drawGridOnBoard(){
        var context = canv.getContext("2d");
            for (var x = 0; x <= width; x += width / boxSize) {
                context.moveTo(0.5 + x, 0);
                context.lineTo(0.5 + x, height);
            }

            for (var x = 0; x <= height; x += height / boxSize) {
                context.moveTo(0, 0.5 + x);
                context.lineTo(width, 0.5 + x);
            }
            context.strokeStyle = "gray";
            context.stroke();
        }
    function updateBoard(withGrid) {
        var context = canv.getContext("2d");
        for(let x = 0; x < boardCells.length; x++) {
            for(let y = 0; y < boardCells[x].length; y++) {
                context.fillStyle = stateColor[boardCells[x][y].state];
                context.fillRect(
                                x * (width / boxSize),
                                y * (height / boxSize), 
                                width / boxSize, height / boxSize);
            }
        }
        if(withGrid) {
            drawGridOnBoard();
        }
    }
</script>
<canvas id="boardCanvas" {height} {width}></canvas>
<style>
    canvas {
        border: 1px solid gray;
        border-top-width: 0px;
        border-left-width: 0px;
    }
</style>