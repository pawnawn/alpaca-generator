<script setup>
    import { ref, reactive } from "vue"

    import CustomCanvas from "./components/CustomCanvas.vue";
    import CustomOption from "./components/CustomOption.vue"

    const alpaca = reactive({
        hair: "./assets/hair/default.png",
        ears: "./assets/ears/default.png",
        eyes: "./assets/eyes/default.png",
        mouth: "./assets/mouth/default.png",
        neck: "./assets/neck/default.png",
        leg: "./assets/leg/default.png",
        accessories: "./assets/accessories/default.png",
        backgrounds: "./assets/backgrounds/default.png",
        nose: "./assets/nose/default.png",
    });

    // Show the hair options by default
    const selectedOption = ref("hair");

    const options = {
        hair: [
            "default",
            "bang",
            "curls",
            "elegant",
            "quiff",
            "short",
        ],

        ears: [
            "default",
            "tilt-backward",
            "tilt-forward",
        ],

        eyes: [
            "default",
            "angry",
            "naughty",
            "panda",
            "smart",
            "star",
        ],

        mouth: [
            "default",
            "astonished",
            "eating",
            "laugh",
            "tongue",
        ],

        neck: [
            "default",
            "bend-backward",
            "bend-forward",
            "thick",
        ],

        leg: [
            "default",
            "bubble-tea",
            "cookie",
            "game-console",
            "tilt-backward",
            "tilt-forward",
        ],

        accessories: [
            "default",
            "earings",
            "flower",
            "glasses",
            "headphone",
        ],

        backgrounds: [
            "default",
        ],

        nose: [
            "default",
        ]
    }

    function getImageSource(option, value) {
        return `./assets/${option}/${value}.png`;
    }

    function randomizeAlpaca() {
        for (const option in alpaca) {
            const randomValue = options[option][Math.floor(Math.random() * options[option].length)]
            alpaca[option] = getImageSource(option, randomValue);
        }
    }
</script>


<template>
    <header>
        <h1>Alpaca generator</h1>
    </header>

    <div id="container">
        <section id="workspace">
            <CustomCanvas 
                v-model:alpaca="alpaca"
                @randomizeAlpaca="randomizeAlpaca"
            ></CustomCanvas>
        </section>
        
        <section id="options">
            <div>
                <h3>Accessorize the alpaca</h3>

                <div class="options-container">
                    <CustomOption
                        v-for="(optionArray, key) in options"
                        :key="key"
                        :data="{
                            id: key,
                            name: 'main-option',
                            value: key,
                        }"
                        v-model="selectedOption"
                    >
                        {{ key }}
                    </CustomOption>
                </div>
            </div>

            <div>
                <h3>Style</h3>

                <div class="options-container">
                    <CustomOption
                        v-for="(value, i) in options[selectedOption]"
                        :key="i"
                        :data="{
                            id: `${selectedOption}-${value}`,
                            name: `${selectedOption}-option`,
                            value: getImageSource(selectedOption, value),
                        }"
                        v-model="alpaca[selectedOption]"
                    >
                        {{ value }}
                    </CustomOption>
                </div>
            </div>
        </section>
    </div>
</template>

<style scoped>
    #container {
        display: flex;
        gap: 5rem;
    }

    #workspace {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    #options {
        display: flex;
        flex-direction: column;
        gap: 2rem;
    }

    .options-container {
        display: flex;
        flex-wrap: wrap;
        gap: 1rem;
    }

    h3 {
        margin-bottom: 1rem;
    }

    h1 {
        padding: 0.5rem 0rem 1rem 0.5rem;
    }


</style>