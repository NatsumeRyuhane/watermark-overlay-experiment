<script setup lang="ts">
import { ref, watch} from "vue";
import Pattern from "./components/Pattern.vue";

// apply a mask to the pattern to leave out the area where the main content will be
const patternClipStyle = ref({});
const pattern = ref<HTMLElement | null>(null);
const mainContent = ref<HTMLElement | null>(null);
let isPatternReady: boolean = false;
let isMainContentReady: boolean = false;

watch(pattern, (newVal) => {
    if (newVal && isMainContentReady) {
        updateClipPath(mainContent.value!);
    }

    if (newVal && !isPatternReady) {
        isPatternReady = true;
        console.log('pattern ready');
    }
});

watch(mainContent, (newVal) => {
    if (newVal && isPatternReady) {
        updateClipPath(mainContent.value!);
    }

    if (newVal && !isMainContentReady) {
        isMainContentReady = true;
        console.log('main content ready');
    }
});


function updateClipPath(mainContent: HTMLElement) {
    let bBox = mainContent.getBoundingClientRect();
    patternClipStyle.value = {
        clipPath: `polygon(
            0px 0px,
            0px 100vh,
            100vw 100vh,
            100vw 0px,
            ${bBox.right}px 0px,
            ${bBox.right}px ${bBox.top}px,
            ${bBox.right}px ${bBox.bottom}px,
            ${bBox.left}px ${bBox.bottom}px,
            ${bBox.left}px ${bBox.top}px,
            ${bBox.right}px ${bBox.top}px,
            ${bBox.right}px 0px,
            0px 0px
        )`
    };
}


</script>

<template>
    <Pattern ref="pattern" :style="patternClipStyle"/>
    <div id="contents">
        <div id="main-content" ref="mainContent">
            <h1>总之是个水印</h1>
        </div>
    </div>
</template>

<style scoped lang="scss">
@import "./config.scss";

pattern {
    width: 100vw;
    height: 100vh;
}

#contents {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
    color: $pattern-color;
}

#main-content {
    width: fit-content;
    height: fit-content;
    padding: 20px;
    background: $background-color;
}

#main-content h1 {
    margin: 0;
    font-size: 8em;
}
</style>
