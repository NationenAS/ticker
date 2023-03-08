<script>
    export let type
</script>

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 9 9">
    <polyline class="{type}" points="1,8 5,1 8,8"/>
</svg>

<style>
svg {
    height: 9px;
}
.pos {
    fill: #fcda51;
}
.neg {
    fill: #333;
    transform: rotate(180deg);
    transform-origin: center;
}
.neu {
    fill: grey
}
</style>