<template lang="pug">
.gamegridrow
    GameGridRowTile(v-for="tile in tiles" :value="tile.value")
</template>

<script>
import GameGridRowTile from "../components/GameGridRowTile.vue";
export default {
    data: () => Object({
        rows: []
    }),
    props: {
        tiles: Array
    },
    mounted () {
        this.rows.map((_, i) => this.tiles.push(this.rows[i]))
    },
    components: {
        GameGridRowTile
    }
}
</script>

<style lang="scss">
.tile {
    display: inline-block;
}
</style>