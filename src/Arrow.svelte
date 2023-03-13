<script>
    export let type
</script>

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 9 9">
    <polyline class="{type}" points="1,8 5,1 8,8"/>
</svg>

<style>
svg {
    height: 0.75em;
    width: auto;
    margin-right: 1px;
    margin-bottom: 1px;
}
.pos {
    fill: #e4ba13;
}
.neg {
    fill: #333;
    transform: rotate(180deg);
    transform-origin: center;
}
.neu {
    fill: grey;
    transform: rotate(90deg);
    transform-origin: center;
}
</style>