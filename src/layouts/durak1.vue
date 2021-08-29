<template>
    <div class="game-area" @click="start()">
        <div class="coloda-my">
            <div v-for="(card) in colodaCard" :key="card" class="card">{{showCard(card)}}</div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            colodaCard: {
                card1: {valueOrig: 0, valuePrint: '6', masti: 'Красное сердце'},
                card2: {valueOrig: 1, valuePrint: '7', masti: 'Красное сердце'},
                card3: {valueOrig: 2, valuePrint: '8', masti: 'Красное сердце'},
                card4: {valueOrig: 3, valuePrint: '9', masti: 'Красное сердце'},
                card5: {valueOrig: 4, valuePrint: '10', masti: 'Красное сердце'},
                card6: {valueOrig: 5, valuePrint: 'B', masti: 'Красное сердце'},
                card7: {valueOrig: 6, valuePrint: 'Q', masti: 'Красное сердце'},
                card8: {valueOrig: 7, valuePrint: 'K', masti: 'Красное сердце'},
                card9: {valueOrig: 8, valuePrint: 'A', masti: 'Красное сердце'},
                card10: {valueOrig: 9, valuePrint: '6', masti: 'Черное сердце'},
                card11: {valueOrig: 10, valuePrint: '7', masti: 'Черное сердце'},
                card12: {valueOrig: 11, valuePrint: '8', masti: 'Черное сердце'},
                card13: {valueOrig: 12, valuePrint: '9', masti: 'Черное сердце'},
                card14: {valueOrig: 13, valuePrint: '10', masti: 'Черное сердце'},
                card15: {valueOrig: 14, valuePrint: 'B', masti: 'Черное сердце'},
                card16: {valueOrig: 15, valuePrint: 'Q', masti: 'Черное сердце'},
                card17: {valueOrig: 16, valuePrint: 'K', masti: 'Черное сердце'},
                card18: {valueOrig: 17, valuePrint: 'A', masti: 'Черное сердце'},
                card19: {valueOrig: 18, valuePrint: '6', masti: 'Буби'},
                card20: {valueOrig: 19, valuePrint: '7', masti: 'Буби'},
                card21: {valueOrig: 20, valuePrint: '8', masti: 'Буби'},
                card21: {valueOrig: 21, valuePrint: '9', masti: 'Буби'},
                card21: {valueOrig: 22, valuePrint: '10', masti: 'Буби'},
                card21: {valueOrig: 23, valuePrint: 'B', masti: 'Буби'},
                card21: {valueOrig: 24, valuePrint: 'Q', masti: 'Буби'},
                card21: {valueOrig: 25, valuePrint: 'K', masti: 'Буби'},
                card21: {valueOrig: 26, valuePrint: 'A', masti: 'Буби'},
                card21: {valueOrig: 27, valuePrint: '6', masti: 'Крести'},
                card21: {valueOrig: 28, valuePrint: '7', masti: 'Крести'},
                card21: {valueOrig: 29, valuePrint: '8', masti: 'Крести'},
                card21: {valueOrig: 30, valuePrint: '9', masti: 'Крести'},
                card21: {valueOrig: 31, valuePrint: '10', masti: 'Крести'},
                card21: {valueOrig: 32, valuePrint: 'B', masti: 'Крести'},
                card21: {valueOrig: 33, valuePrint: 'Q', masti: 'Крести'},
                card21: {valueOrig: 34, valuePrint: 'K', masti: 'Крести'},
                card21: {valueOrig: 35, valuePrint: 'A', masti: 'Крести'},
            },
        }
    },
    methods: {
        showCard(card) {
            return card.valuePrint
        },
    }
}
</script>