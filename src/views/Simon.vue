<template>
    <div class="wrapper">
        <div class="returnButton">
            <router-link to="/" >
                <button>back</button>
            </router-link>
        </div>
        <hr>
        <h1>Simon Says</h1>
        <div class="game">
            <div class="game-board">
                <div class="tile red" id="1" v-on:click="clickElement(1)"></div>
                <div class="tile blue" id="2" v-on:click="clickElement(2)" ></div>
                <div class="tile orange" id="3" v-on:click="clickElement(3)"></div>
                <div class="tile green"  id="4" v-on:click="clickElement(4)"></div>
            </div>
            <div class="game-bar">
                <div class="game-info">
                    <h2>Раунд: <span data-round="0">{{round}}</span></h2>
                    <button data-action="start" v-on:click="startGame" :disabled="button.isDisabled">Начать</button>
                    <p id="lose" data-action="lose">Извините, вы проиграли !</p>
                </div>
                <div class="game-options">
                    <h2>Сложность</h2>
                    <input type="radio" v-model="level" value="1500" :disabled="radioInput.isDisabled">Легкий<br>
                    <input type="radio" v-model="level" value="1000" :disabled="radioInput.isDisabled">Средний<br>
                    <input type="radio" v-model="level" value="400" :disabled="radioInput.isDisabled">Тяжелый<br>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
    export default {
        data() {
            return {
                simonNumberSet: [],
                userNumberSet: [],
                numberClick: 0,
                level: '1500',
                round: 1,
                radioInput: {
                    isDisabled: false
                },
                button: {
					isDisabled: false
                },
                audio: {
                    src: ""
                },
                gameIsReady: false
            }
        },
        methods: {
            startGame() {
                this.radioInput.isDisabled = true
				this.button.isDisabled = true
                for (let i = 0; i < this.round; i++) {
                    this.simonNumberSet.push(Math.floor(Math.random() * (5 - 1) + 1))
				}
                let  i = 1;
                for(let elem of this.simonNumberSet) {
					setTimeout( () => {
						let element = document.getElementById(String(elem))
						this.playSound(elem)
						element.classList.add('active')
                        setTimeout(() => {element.classList.remove('active')}, 300)
                    }, this.level * i++)
                }
                this.gameIsReady = true
            },
			clickElement(val) {
                this.playSound(val)
                if(this.gameIsReady) {
                    this.userNumberSet.push(val)
                    if(val === this.simonNumberSet[this.numberClick] && this.userNumberSet.length === this.simonNumberSet.length) {
						this.reset(false)
                        this.round++
                        this.startGame()
                    }
                    else if (val !== this.simonNumberSet[this.numberClick]) {
						this.reset(true)
                        document.getElementById('lose').style.display = 'block'
                        setTimeout(() => {document.getElementById('lose').style.display = 'none'}, 5000)
                    }
                    else {
						this.numberClick++
                    }
                }
			},
            reset(full) {
				this.simonNumberSet = []
				this.userNumberSet = []
				this.numberClick = 0
                if(full) {
                    this.round = 1
					this.radioInput.isDisabled = false
					this.button.isDisabled = false
					this.gameIsReady = false
                }
            },
            playSound(val) {
				let audio = new Audio(require(`../assets/audio/${val}.mp3`))
				audio.play()
            }
		}
    }
</script>

<style scoped lang="sass">
    body
        color: #333
        -webkit-user-select: none
        -moz-user-select: none
        -ms-user-select: none
        -o-user-select: none
        user-select: none
    h1
        margin: 1em 0 2em
        text-align: center

    p[data-action="lose"]
        display: none

    .clearfix
        width: 100%
        clear: both
    .wrapper
        width: 580px
        margin: 0 auto
    .game
        display: flex
        flex-wrap: wrap
        flex-direction: row
        text-align: center
    .game-board
        width: 65%
        display: flex
        flex-wrap: wrap
        flex-direction: row
    .game-bar
        width: 35%
        position: relative
        margin-top: -100px
    .red
        background: #FF5643
        width: 150px
        height: 150px
        margin-right: 5px
        border-radius: 150% 5px 5px 5px
        filter: contrast(0.4)
    .blue
        background: dodgerblue
        width: 150px
        height: 150px
        border-radius: 5px 150% 5px 5px
        filter: contrast(0.4)
    .orange
        background: orange
        width: 150px
        height: 150px
        margin: 5px 5px 0 0
        border-radius: 5px 5px 5px 150px
        filter: contrast(0.4)
    .green
        background: #BEDE15
        width: 150px
        height: 150px
        margin: 5px 5px 0 0
        border-radius: 5px 5px 150% 5px
        filter: contrast(0.4)
    .red:active, .blue:active, .green:active, .orange:active
        filter: contrast(2)
    .game-info
        margin-top: 90px
    .game-info button
        width: 5em
        box-sizing: border-box
        font-size: 1.4em
        -webkit-border-radius: 10px 10px 10px 10px
        border-radius: 10px 10px 10px 10px
        background: #6DABE8
        border: none
        padding: 0.3em 0.6em
        outline: none
    .game-info button:hover
        background: #78BCFF

    input[type="radio"], button, .tile
        cursor: pointer
    input[type="radio"]
        position: relative
        width: 1.2em
        height: 1.2em
        top: 4px
        background: #c6c6c6
    .tile:active, button:active
        transform: scale(0.98)
    .returnButton
        text-align: center
    .active
        transform: scale(0.98)
        filter: contrast(2)
</style>