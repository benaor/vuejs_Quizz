<template>
    <section class="score">
        player
        <span>{{ winCount }}</span> |
        <span>{{ loseCount }}</span> Computer
    </section>
</template>

<script lang="ts">
import { defineComponent } from "vue"

let props = {
    winCount: Number,
    loseCount: Number
}

export default defineComponent({
    props,

})

</script>

<style lang="scss">
section.score {
    border-bottom: 1px solid black;
    padding: 14px;
    font-size: 18px;

    span {
        padding: 8px;
        font-weight: bold;
        border: 1px solid black;
    }
}
</style>