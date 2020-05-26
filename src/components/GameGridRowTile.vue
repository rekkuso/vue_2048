<template lang="pug">
.tile(:class="'_' + value")
</template>
<script>
export default {
    props: {
        value: Number
    }
}
</script>

<style lang="scss">
.tile {
    font-size: 4vw;
    width: 3em;
    height: 3em;
    margin: .1em;
    border: 4px solid #FBE6CC;
    border-radius: 4px;
    cursor: pointer;
}
.value {
    position: relative;
    top: 50%;
    transform: translateY(-50%);
    font-family: sans-serif;
}
._2, ._4, ._8, ._16, ._32, ._64, ._128, ._256, ._512, ._1024, ._2048 {
    transition: 0.25s;
    transition-delay: 0.05s;
    &:after {
        position: absolute;
        transform: translate(-50%, 70%);
        font-family: monospace;
        font-weight: bold;
    }
}

// Tile colors and animations
$color0:rgb(255, 247, 238);
$color2: #FBE6CCff;
$color4: #F7CD99ff;
$color8: #F4B466ff;
$color16: #F09B33ff;
$color32: #EC8200ff;
$color64: #ED2F00ff;
$color128: #BE2600ff;
$color256: #8E1C00ff;
$color512: #5F1300ff;
$color1024: #2F0900ff;
$color2048: #000000ff;

$content0: "";
$content2: "2";
$content4: "4"; 
$content8: "8";
$content16: "16";
$content32: "32";
$content64: "64";
$content128: "128";
$content256: "256";
$content512: "512";
$content1024: "1024";
$content2048: "2048";


@keyframes example {
  0%   {transform: scale(1,1)} 
  50%  {transform: scale(1.07,1.07)} 
  100% {transform: scale(1,1)}
}

._0 {
    animation-name: example;
    animation-duration: 0.3s;
    background-color: $color0;
}
._2 {
    background-color: $color2;
    &:after {
        content: $content2;
    }
}
._4 {
    background-color: $color4;
    &:after {
        content: $content4;
    }
}
._8 {
    background-color: $color8;
    &:after {
        content: $content8;
    }
}
._16 {
    background-color: $color16;
    &:after {
        content: $content16;
    }
}
._32 {
    background-color: $color32;
    &:after {
        content: $content32;
    }
}
._64 {
    background-color: $color64;
    &:after {
        content: $content64;
    }
}
._128 {
    color: #FFF;
    background-color: $color128;
    &:after {
        content: $content128;
    }
}
._256 {
    color: #FFF;
    background-color: $color256;
    &:after {
        content: $content256;
    }
}
._512 {
    color: #FFF;
    background-color: $color512;
    &:after {
        content: $content512;
    }
}
._1024 {
    color: #FFF;
    background-color: $color1024;
    &:after {
        content: $content1024;
    }
}
._2048 {
    color: #FFF;
    background-color: $color2048;
    transform: scale(1.5, 1.5);
    box-shadow: 2px 2px #CCC;
    &:after {
        content: $content2048;
    }
}
</style>