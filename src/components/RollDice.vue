<script setup>
import { ref } from 'vue';

const emit = defineEmits(['throw']);

const diceValues = ref([])

const NUMBER_OF_DICE = 5

const rollDice = () =>{    
    diceValues.value = []
    
    for (let index = 0; index < NUMBER_OF_DICE; index++) {
        const randomNumber = Math.ceil(Math.random() * 6);

        diceValues.value.push(randomNumber);
    };
}

const emitEvent = () =>{
    rollDice();
    emit('throw', diceValues.value);
}

    // Props-toegang: defineProps retourneert een object. Overweeg om via destructuring de daadwerkelijke waarde te halen,
    // bijvoorbeeld: const { throwCount } = defineProps({ throwCount: Number }).
</script>

<template>
    <button @click="emitEvent">Throw</button>
</template>
