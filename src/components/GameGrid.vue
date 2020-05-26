<template lang="pug">
#gamegrid
    GameGridRow.row(v-for="row in rows" :tiles="row")
</template>

<script>
import GameGridRow from "../components/GameGridRow.vue"
export default {
    props: {
        rows: Array
    },
    components: {
        GameGridRow
    }
}
</script>

<style lang="scss">

</style>