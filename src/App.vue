<script setup>
import { ref } from 'vue';
import ScoreTable from './components/ScoreTable.vue';
import RollDice from './components/RollDice.vue';

const diceRolled = ref([1, 2, 3, 4, 5])



</script>

<template>
    <div>
        <RollDice v-model="diceRolled"/>
        <br>
        {{ diceRolled }}
        <br>
        <br>
        <ScoreTable :dice-rolled="diceRolled"/>
    </div>

</template>

<style scoped>

</style>
