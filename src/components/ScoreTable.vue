<script setup>
import { computed } from 'vue';

defineProps({
    diceRolled: Array,
})
const diceRolled = [];

const computedDiceCount = computed(() => { 
  const diceCount = {
    1 : 0,
    2 : 0,
    3 : 0,
    4 : 0,
    5 : 0,
    6 : 0,
  };

  diceRolled.value.forEach(value => diceCount[value]++);

  return diceCount;
})





</script>

<template>
    <div>
        <p>{{ diceRolled }}</p>
        <table>
            <thead>

            </thead>
            <tbody v-for="(key, value) in computedDiceCount">
                <tr>
                <td>{{ key }}</td><td>{{ value }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>