<template>
    <div class="game-area" @click="start()">
        <div class="container">
            <div class="coloda-bot">
                <div v-for="card in botCards" :key="card" class="card">{{showCard(card)}}</div>
            </div>
            <div class="game-area-fight">
                <div v-for="card in fightCards" :key="card" class="card-fight">{{showCard(card)}}</div>
            </div>
            <div class="nav-button attack" v-if="showButtonAttack()">
                <button class="btn-choose" @click="changeHod()">Бито</button>
            </div>
            <div class="nav-button def" v-else-if="showButtonDef()">
                <button class="btn-choose" @click="take(myCards)">Беру</button>
            </div>
            <div class="coloda-my">
                <div v-for="card in myCards" :key="card" @click="addCard(card)" class="card">{{showCard(card)}}</div>
            </div>
        </div>
    </div>
</template>


<script>
import '@/utils/durakGame.js'
export default {
    data() {
        return {
            colodaCard: {
                card0: {valueOrig: 0, valuePrint: '6', masti: 'Красное сердце'},
                card1: {valueOrig: 1, valuePrint: '7', masti: 'Красное сердце'},
                card2: {valueOrig: 2, valuePrint: '8', masti: 'Красное сердце'},
                card3: {valueOrig: 3, valuePrint: '9', masti: 'Красное сердце'},
                card4: {valueOrig: 4, valuePrint: '10', masti: 'Красное сердце'},
                card5: {valueOrig: 5, valuePrint: 'B', masti: 'Красное сердце'},
                card6: {valueOrig: 6, valuePrint: 'Q', masti: 'Красное сердце'},
                card7: {valueOrig: 7, valuePrint: 'K', masti: 'Красное сердце'},
                card8: {valueOrig: 8, valuePrint: 'A', masti: 'Красное сердце'},
                card9: {valueOrig: 9, valuePrint: '6', masti: 'Черное сердце'},
                card10: {valueOrig: 10, valuePrint: '7', masti: 'Черное сердце'},
                card11: {valueOrig: 11, valuePrint: '8', masti: 'Черное сердце'},
                card12: {valueOrig: 12, valuePrint: '9', masti: 'Черное сердце'},
                card13: {valueOrig: 13, valuePrint: '10', masti: 'Черное сердце'},
                card14: {valueOrig: 14, valuePrint: 'B', masti: 'Черное сердце'},
                card15: {valueOrig: 15, valuePrint: 'Q', masti: 'Черное сердце'},
                card16: {valueOrig: 16, valuePrint: 'K', masti: 'Черное сердце'},
                card17: {valueOrig: 17, valuePrint: 'A', masti: 'Черное сердце'},
                card18: {valueOrig: 18, valuePrint: '6', masti: 'Буби'},
                card19: {valueOrig: 19, valuePrint: '7', masti: 'Буби'},
                card20: {valueOrig: 20, valuePrint: '8', masti: 'Буби'},
                card21: {valueOrig: 21, valuePrint: '9', masti: 'Буби'},
                card22: {valueOrig: 22, valuePrint: '10', masti: 'Буби'},
                card23: {valueOrig: 23, valuePrint: 'B', masti: 'Буби'},
                card24: {valueOrig: 24, valuePrint: 'Q', masti: 'Буби'},
                card25: {valueOrig: 25, valuePrint: 'K', masti: 'Буби'},
                card26: {valueOrig: 26, valuePrint: 'A', masti: 'Буби'},
                card27: {valueOrig: 27, valuePrint: '6', masti: 'Крести'},
                card28: {valueOrig: 28, valuePrint: '7', masti: 'Крести'},
                card29: {valueOrig: 29, valuePrint: '8', masti: 'Крести'},
                card30: {valueOrig: 30, valuePrint: '9', masti: 'Крести'},
                card31: {valueOrig: 31, valuePrint: '10', masti: 'Крести'},
                card32: {valueOrig: 32, valuePrint: 'B', masti: 'Крести'},
                card33: {valueOrig: 33, valuePrint: 'Q', masti: 'Крести'},
                card34: {valueOrig: 34, valuePrint: 'K', masti: 'Крести'},
                card35: {valueOrig: 35, valuePrint: 'A', masti: 'Крести'},
            },
            myCards: [],
            fightCards: [],
            botCards: [],
            hod: true,
            myHod: true,
            currentAtackCard: 0,
            game: false,
            cozir: 36,
            beginRaund: true,
        }
    },
    methods: {
        showCard(card) {
            return `${card.valuePrint} ${card.masti}`
        },
        showButtonAttack() {
            return (this.game && this.myHod)
        },
        showButtonDef() {
            return (this.game && !this.myHod)
        },
        changeHod() {
            this.beginRaund = true
            this.fightCards = []
            this.hod = !this.hod
            this.myHod = !this.myHod
            this.botHod_atack()
        },
        addCard(card_obj) { // Выбор по какому правилу будет добавлена карта на стол (Зависит от Атаки или Защиты)
            if (this.myHod) this.addCard_fight(card_obj)
            else this.addCard_def(card_obj)
        },
        addCard_fight(card_obj) { // Мой ход (Атака)
            if (this.hod) {
                if (this.sameWeight_rool(card_obj) || this.beginRaund) {
                    this.fightCards.push(this.deleteCard(this.myCards, card_obj.valueOrig))
                    this.hod = !this.hod
                    this.beginRaund = false
                    this.botHod_def()
                }
                else console.log('Выберете другую карту')
            }
            
        },
        addCard_def(card_obj) { // Мой ход (Защита)
            let attackCard = this.fightCards[this.currentAtackCard]
            if (this.higher_rool(card_obj, attackCard) && this.masti_rool(card_obj, attackCard)) {
                this.fightCards.push(card_obj)
                this.currentAtackCard += 2
                this.mindBot_atack() // После того как я отбился очередной раз вызывается ход бота
            }
        },
        deleteCard(array, card) { // Удаляем карту из массива, при этом возвращая её (array - array, card - origValue)
            for (let i = 0; i < array.length; i++) {
                if (array[i].valueOrig === card) {
                    let fightCard = array.splice(i, 1)[0]
                    return fightCard
                }
            }
        },
        take(array) {
            while (this.fightCards.length > 0) {
                array.push(this.deleteCard(this.fightCards, this.fightCards[0].valueOrig))
            }
            this.currentAtackCard = 0
            this.beginRaund = true
            if (!this.myHod) {
                this.botHod_atack()
            }
        },
        mindBot_def() { // Мозг бота (Защита)
            if (this.myHod || !this.hod) {
                let perhapsCard = []
                let min = 100
                // for (let i = 0; i < this.botCards.valueOrig.length; i++) { // Заполнение массива возможных карт чтобы отбится 
                //     let currentCard = this.botCards.valueOrig[i] // Очередная карта из колоды карт бота
                //     let attackCard = this.fightCards[this.currentAtackCard]
                //     if (currentCard > attackCard && this.masti_rool_def(currentCard, attackCard)) {
                //         perhapsCard.push(currentCard)
                //     }
                // }
                for (let i = 0; i < this.botCards.length; i++) {
                    let currentCard = this.botCards[i]
                    let attackCard = this.fightCards[this.currentAtackCard]
                    if (this.higher_rool(currentCard, attackCard) && this.masti_rool(currentCard, attackCard)) {
                        perhapsCard.push(currentCard)
                    }
                }
                for (let i = 0; i < perhapsCard.length; i++) { // Выбираем наилучшую карту для отбоя
                    if (perhapsCard[i].valueOrig < min) {
                        min = perhapsCard[i].valueOrig
                    }
                }
                if (perhapsCard.length > 0) { // Добавляем карту на стол
                    this.fightCards.push(this.deleteCard(this.botCards, min))
                    this.currentAtackCard += 2
                }
                else {
                    console.log('Беру')
                    this.beginRaund = true
                    this.take(this.botCards)
                }
                this.hod = !this.hod
            }
        },
        mindBot_atack() { // Мозг бота (Атака)
            if (!this.myHod || this.hod) {
                this.botChooseCard()
                if (this.beginRaund) {
                    this.beginRaund = false
                }
            }
        },
        botChooseCard() { // Бот выбирает карту чтобы сходить
            // let card = this.deleteCard(this.botCards, this.botCards[0].valueOrig)
            // this.fightCards.push(card)
            for (let i = 0; i < this.botCards.length; i++) {
                let card = this.botCards[i] // Очередная карта бота
                if (this.sameWeight_rool(card) || this.beginRaund) {
                    this.fightCards.push(this.deleteCard(this.botCards, card.valueOrig))
                    return
                }
            }
        },
        botHod_def() { // Задаем ожидание перед ходом бота
            setTimeout(() => {this.mindBot_def()}, 700)
        },
        botHod_atack() {
            setTimeout(() => {this.mindBot_atack()}, 700)
        },
        higher_rool(currentCard, attackCard) {
            console.log(`current - ${currentCard}, attack ${attackCard}`)
            let CC = this.decrementCard(currentCard.valueOrig)
            let AC = this.decrementCard(attackCard.valueOrig)
            return CC > AC
        },
        masti_rool(currentCard, attackCard) {
            return (currentCard.masti === attackCard.masti)
        },
        sameWeight_rool(card_obj) {
            let myCard = this.decrementCard(card_obj.valueOrig)
            for (let i = 0; i < this.fightCards.length; i++) {
                let tableCard = this.fightCards[i].valueOrig
                tableCard = this.decrementCard(tableCard)
                console.log(myCard + '=' + tableCard)
                if (myCard === tableCard) {
                    return true
                }
            }
            return false
        },
        // passedCard(card) {
        // },
        // cardOnCard() {
        //     let fightCard = document.querySelector('.card-fight')
        //     fightCard.style
        // },
        start() { // Отслеживаем начало игры
            // setInterval(() => {this.sameWeight_rool_atac()}, 500)
            if (!this.game) {
                this.pullHand(this.myCards)
                this.pullHand(this.botCards)
                this.game = true
            }
        },
        pullHand(hand) { // Заполняем руку игрока картами
            for (let i = 0; i < 6; i++) {
                let randomCard = Math.floor(Math.random() * 36)
                let key = `card${randomCard}`
                let curCard = this.deleteReturn_obj(this.colodaCard, key)
                hand.push(curCard)
            }
        },
        deleteReturn_obj(obj, key) {
            let card = obj[key]
            delete obj.key
            return card
        },
        // valueToCard(hand_obj) { // Изменяем массив чисел в привычные значения карт 
        //     for (let i = 0; i < hand_obj.valueOrig.length; i++) {
        //         let currentCard = hand_obj.valueOrig[i]
        //         let currentCard_num = this.decrementCard(currentCard)
        //         let currentCard_masti = this.findMasti(currentCard)
        //         console.log('Текущее значение', currentCard, 'Его интерпретация', this.cards[currentCard_num], 'Масть', currentCard_masti)
        //         hand_obj.valuePrint.push(this.cards[currentCard_num])
        //     }
        // },
        // rools(card) {
        //     if (1) {
        //         console.log(1)
        //     }
        // },
        // findMasti(card) {
        //     let m = Math.floor(card / 9)
        //     return this.masti[m]
        // },
        decrementCard(card) { // Понижаем значение карты к первой девятке
            while (card >= 9) {
                card -= 9
            }
            return card
        },
        // incrementCard(card, masti) {

        // },
        // sameWeight_rool_atac(card) { // Правило: Подкидывать карты можно только те, что уже лежат на столе
        //     let addPerhapsCard = []
        //     for (let i = 0; i < this.fightCards.length; i++) {
        //         let currentCard = this.decrementCard(this.fightCards[i])
        //         for (let j = 1; j < 5; j++) {
        //             addPerhapsCard.push(currentCard)
        //             currentCard += 9
        //         }
        //     }
        // },
        // masti_rool_def(cardHand, cardTable) { // Определяет масти
        //     let mastiHand = Math.floor(cardHand / 9)
        //     let mastiTable = Math.floor(cardTable / 9)
        //     if (mastiHand === mastiTable) {
        //         return true
        //     }
        //     else return false
        // },
        
    },
}
</script>


<style lang="less" scoped>
    .main-title {
        font-size: 30px;
        text-align: center;
    }
    .game-area {
        height: 100vh;
        // overflow: hidden;
        &-fight {
            display: flex;
            justify-content: center;
            height: 400px;
            margin-top: 80px;
        }
    }
    .coloda {
        &-my {
            display: flex;
            justify-content: center;
            align-items: flex-end;
            height: 300px;
        }
        &-bot {
            display: flex;
            justify-content: center;
            align-items: flex-start;
            height: 150px;
            overflow: hidden;
        }
    }
    .card {
        width: 200px;
        height: 300px;
        background-color: aqua;
        margin: 0 10px;
        transition: .3s;
        &-fight {
            width: 200px;
            height: 300px;
            background-color: bisque;
            box-shadow: 1px 1px 2px rgba(0, 0, 0, .2), 1px 1px 2px rgba(0, 0, 0, .2);
            transition: .3s;
            margin: 0 10px;
        }
        &:hover {
            margin-bottom: 50px;
        }
    } 
</style>