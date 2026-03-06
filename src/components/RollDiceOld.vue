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

</script>

<template>
    <button @click="emitEvent">Throw</button>
    <br>
    {{ diceValues }}
</template>
