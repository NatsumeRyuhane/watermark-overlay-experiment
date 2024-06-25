<script setup lang="ts">

</script>

<template>
<div class="wrapper">
    <div class="background"></div>
</div>
</template>

<style scoped lang="scss">
@import "../config.scss";

body, html {
    margin: 0;
    padding: 0;
    overflow: hidden;
    height: 100%;
}

.wrapper {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    overflow: hidden;
}

.background {
    position: absolute;
    top: 0;
    left: 0;
    width: 200%;
    height: 200%;
    background: repeating-linear-gradient(
            135deg,
            $pattern-color,
            $pattern-color $pattern-width,
            $occlusion-color $pattern-width,
            $occlusion-color calc($pattern-width * 2 / $pattern-density)
    );
    animation: moveBackground 20s linear infinite;
}

@keyframes moveBackground {
    from {
        transform: translate(-50%, -50%);
    }
    to {
        transform: translate(0%, 0%);
    }
}
</style>