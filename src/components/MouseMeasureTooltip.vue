<template>
    <div 
    class="tooltip"
    :style="`left: ${positionX}px; top: ${positionY}px`"
    >
        {{ text }}
    </div>
</template>

<script>
export default {
    props: {
        text: String,
        positionX: Number,
        positionY: Number
    }
}
</script>

<style scoped>

.tooltip {
    position: absolute;
    background: rgba(0, 0, 0, 0.5);
    border-radius: 4px;
    color: white;
    padding: 4px 8px;
    opacity: 0.7;
    white-space: nowrap;
    font-size: 12px;
    z-index: 30;
}

</style>