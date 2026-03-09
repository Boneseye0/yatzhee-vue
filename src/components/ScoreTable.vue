<script setup>
import { computed } from 'vue';

let TophalfBonus = 30

const diceRolled = defineModel()

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
});

const checkKinds = (size) => {
    for (const key in computedDiceCount.value) {
        if (computedDiceCount.value[key] >= size) return true
    }
    return false;
};

const checkFullHouse = () => {
    let double = false;
    let triple = false;
    for (const key in computedDiceCount.value) {
        if (computedDiceCount.value[key] === 2) double = true;
        if (computedDiceCount.value[key] === 3) triple = true;
        if (computedDiceCount.value[key] === 5) double = true, triple = true;
    }
    return double && triple;
};

const checkStraights = (size) => {
    
    let straightCheck = 0;
    for (const key in computedDiceCount.value) {
        if (computedDiceCount.value[key] > 0) {straightCheck++} else {straightCheck = 0}
        if (straightCheck === size) {return true};
    }
    return false;
};

const computedTopHalfScore = computed(() => {
       const topHalfScore = {
        1 : 0,
        2 : 0,
        3 : 0,
        4 : 0,
        5 : 0,
        6 : 0,
        };
        
        Object.keys(computedDiceCount.value).forEach(value =>
        topHalfScore[value] = computedDiceCount.value[Number(value)] * Number(value))
        return topHalfScore;
});

const topHalfSubtotal = computed(() => {
    let score = 0
    Object.keys(computedTopHalfScore.value).forEach(key => score += computedTopHalfScore.value[Number(key)])
    if (score >= TophalfBonus) score += 35;
    return score
});

const totalDiceValue = () => {
       return diceRolled.value.reduce((total, num) => total+num, 0)
};

const computedThreeOfAKind = computed(() => {
    return checkKinds(3) ? totalDiceValue() : 0;
});

const computedFourOfAKind = computed(() => {
    return checkKinds(4) ? totalDiceValue() : 0;
});

const computedFiveOfAKind = computed(() => {
    return checkKinds(5) ? 50 : 0;
});

const computedFullHouse = computed(() => {
    return checkFullHouse() ? 25 : 0;
});

const computedSmallStraight = computed(() => {
    return checkStraights(4) ? 30 : 0;
});

const computedFullStraight = computed(() => {
    return checkStraights(5) ? 40 : 0;
});

const computedChance = computed(() => {
    return totalDiceValue();
});

const botHalfSubTotal = computed(() => {
    return  computedThreeOfAKind.value
            + computedFourOfAKind.value
            + computedFullHouse.value
            + computedSmallStraight.value
            + computedFullStraight.value
            + computedChance.value
            + computedFiveOfAKind.value;
});

const computedTotalScore = computed(() => {
    return topHalfSubtotal.value + botHalfSubTotal.value;
})

</script>

<template>
    <div>
        <table>
            <thead>
                <tr>
                    <td>ogen</td><td>stenen</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(property, key) in computedDiceCount">
                    <td>{{ key }}</td><td>{{ property }}</td>
                </tr>
            </tbody>
        </table>
        <br>
        <table>
            <thead>
                <tr>
                    <td colspan=2>Bovenste Helft</td>
                </tr>
                <tr>
                    <td>score</td><td>punten</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(prop, key) in computedTopHalfScore">
                    <td>{{ key }}</td><td>{{ prop }}</td>
                </tr>
                <tr>
                    <td>subtotaal</td><td>{{ topHalfSubtotal }}</td><p>{{ topHalfSubtotal >= TophalfBonus ? "35 bonus punten" : "" }}</p>
                </tr>
                <br>
                <tr>
                    <td colspan="2">Onderste Helft</td>
                </tr>            
                <tr>
                    <td>Three of a kind</td><td>{{ computedThreeOfAKind }}</td>
                </tr>
                <tr>
                    <td>Four of a kind</td><td>{{ computedFourOfAKind }}</td>
                </tr>
                <tr>
                    <td>Full House</td><td>{{ computedFullHouse }}</td>
                </tr>
                <tr>
                    <td>Kleine Straat</td><td>{{ computedSmallStraight }}</td>
                </tr>
                <tr>
                    <td>Grote Straat</td><td>{{ computedFullStraight }}</td>
                </tr>
                <tr>
                    <td>Kans</td><td>{{ computedChance }}</td>
                </tr>
                <tr>
                    <td>Yahtzee!</td><td>{{ computedFiveOfAKind }}</td>
                </tr>
                <tr><td>subtotaal</td><td>{{ botHalfSubTotal }}</td></tr>
                <tr><td>totaal</td><td>{{ computedTotalScore }}</td></tr>
            </tbody>
        </table>
    </div>
</template>