<script>
    import { onMount } from 'svelte';

    // assume square map
    let canvas;
    let ctx;
    let x = 0;
    let y = 0;
    $: drawCursor(x, y);
    const unit = 5;
    const ratio = 50;
    let mapRatio = 1;

    onMount(() => {
		ctx = canvas.getContext('2d');
        mapRatio = canvas.height / (unit * ratio);

        // draw map border
        ctx.lineWidth = 1;
        ctx.strokeRect(0, 0, 250, 250);
        drawCursor();
	});

    function drawCursor(currentX, currentY) {
        if (ctx) {
            ctx.fillRect(currentX * mapRatio, currentY * mapRatio, unit * mapRatio, unit * mapRatio);
        }
    }

    // watch for movement
    function handleKeydown(event) {
        const keyCode = event.keyCode;

        if (keyCode == 65 || keyCode == 37) { // left or a
            event.preventDefault();
            if (x - unit >= 0) {
                x -= unit;
            } else if (x - unit < 0) {
                x = 0;
            }
        }
        
        if (keyCode == 68 || keyCode == 39) { // right or d
            event.preventDefault();
            if (x + unit <= unit * ratio) {
                x += unit;
            } else if (x + unit > unit * ratio) {
                x = unit * ratio;
            }
        }
        
        if (keyCode == 87 || keyCode == 38) { // up or w
            event.preventDefault();
            if (y - unit >= 0) {
                y -= unit;
            } else if (y - unit < 0) {
                y = 0;
            }
        } 
        
        if (keyCode == 83 || keyCode == 40) { // down or s
            event.preventDefault();
            if (y + unit <= unit * ratio) {
                y += unit;
            } else if (x + unit > unit * ratio) {
                y = unit * ratio;
            }
        }
    }
</script>

<svelte:window on:keydown={handleKeydown}/>

<h1>Welcome to Svelte Sandbox</h1>
<p>use arrow keys or wasd to move the cursor around</p>
<canvas
	bind:this={canvas}
	width={250}
	height={250}
    class='ml-1'
></canvas>