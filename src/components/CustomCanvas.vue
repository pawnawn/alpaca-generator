<script setup>
    import { onMounted, ref, watch } from 'vue';

    const props = defineProps({
        alpaca: Object,
    })

    const emit = defineEmits(["randomizeAlpaca"])

    const canvas = ref(null);
    let ctx = undefined;

    const drawingOrder = [ // Just to make sure the layers are drawn in the correct order
        "backgrounds", "leg", "neck", "nose", "mouth", "ears", "hair", "accessories", "eyes",
    ];

    async function drawAlpaca() {
        ctx.clearRect(0, 0, canvas.value.width, canvas.value.height);

        // Iterate over the drawing order
        for (const option of drawingOrder) {
            const image = new Image();

            // Set the source of the image
            image.src = props.alpaca[option];

            // Wait for the image to load
            await new Promise((resolve) => {
                image.onload = () => resolve();
                image.onerror = () => {
                    console.log("Failed to load image: ", image.src);
                }
            });

            // Draw the image onto the canvas at position (0, 0)
            const widthRatio = image.width / canvas.value.width;
            const heightRatio = image.height / canvas.value.height;
            
            ctx.drawImage(
                image,
                0, 0,
                image.width, image.height,
                0, 0,
                image.width * widthRatio, image.height * heightRatio,
            );
        }
    }

    function downloadImage() {
        // https://stackoverflow.com/a/50300880

        const link = document.createElement("a");
        link.download = "alpaca.png";

        link.href = canvas.value.toDataURL("image/png");
        link.click(); 
    }

    watch(props.alpaca, drawAlpaca);

    onMounted(() => {
        ctx = canvas.value.getContext("2d");
    
        drawAlpaca();
    });
</script>

<template>
    <canvas ref="canvas" width="720" height="720"></canvas>

    <div id="button-container">
        <button @click="emit('randomizeAlpaca')">Random</button>
        <button @click="downloadImage">Download</button>
    </div>
</template>

<style scoped>
    canvas {
        width: 30rem;
        height: 30rem;
    }

    #button-container {
        display: flex;
        justify-content: center;
        gap: 1rem;
    }

    button {
        cursor: pointer;
    }

    button {
        padding: 0.5rem 1rem;
        border-radius: 0.5rem;
        border: unset;
        background-color: #ff002b;
        color: white;

        transition: 0.25s all;
    }

    button:active {
        scale: 0.95;
    }
</style>