<template>
  <div id="app">
    <button id="startGame" @click="startGame">jouer</button>
    <button id="resetButton" @click="handleReset">reset</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
const puissance4 = [
  [0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0],
  [0, 0, 0, 0, 0, 0, 0],
]
let taille = puissance4.length - 1
let win: boolean = false

const player1 = 1
const player2 = 2
let currentPlayer = player1

export default Vue.extend({
  name: 'IndexPage',
  data() {
    return {}
  },
  mounted() {},
  methods: {
    createPlayground() {
      const app = document.getElementById('app')
      const startGame = document.getElementById('startGame')
      if (startGame === null || app === null) {
        return
      }
      startGame.style.display = 'none'
      const playground = document.createElement('div')
      const stopGame = document.createElement('button')
      stopGame.classList.add('stopGame')
      stopGame.innerHTML = 'Stop Game'
      playground.classList.add('playground')
      app.appendChild(playground)
      app.appendChild(stopGame)
      stopGame.addEventListener('click', () => {
        app.removeChild(playground)
        app.removeChild(stopGame)
        startGame.style.display = 'block'
      })
      for (let i = 0; i < 7; i++) {
        const button = document.createElement('button')
        button.classList.add('button')
        button.innerHTML = 'ajouter'
        button.setAttribute('data-column', i as any)
        const column = document.createElement('div')
        column.classList.add('column')
        column.appendChild(button)
        button.addEventListener('click', () => {
          if (win) {
            return
          }

          const dataColumn = parseInt(
            button.getAttribute('data-column') as string
          )

          for (let m = 0; m < 6; m++) {
            if (puissance4[taille][dataColumn] === 0) {
              puissance4[taille][dataColumn] = currentPlayer
              const circle = document.querySelector(
                `[data-row="${taille}"][data-column="${dataColumn}"]`
              )
              if (circle === null) {
                return
              }
              circle.classList.add(`player${currentPlayer}`)
              this.switchPlayer()
              taille--
              win = this.checkWin()
              break
            } else {
              taille--
            }
          }
          taille = puissance4.length - 1
        })

        for (let j = 0; j < 6; j++) {
          const cell = document.createElement('div')
          const circle = document.createElement('div')
          cell.classList.add('cell')
          circle.classList.add('circle')
          circle.setAttribute('data-row', j as any)
          circle.setAttribute('data-column', i as any)
          cell.appendChild(circle)
          column.appendChild(cell)
        }

        playground.appendChild(column)
      }
    },
    checkWin() {
      // 1. vérifier les lignes
      for (let i = 0; i < 6; i++) {
        for (let j = 0; j < 4; j++) {
          if (
            puissance4[i][j] === currentPlayer &&
            puissance4[i][j + 1] === currentPlayer &&
            puissance4[i][j + 2] === currentPlayer &&
            puissance4[i][j + 3] === currentPlayer
          ) {
            alert(`Joueur ${currentPlayer} a gagné`)
            return true
          }
        }
      }

      // 2. vérifier les colonnes
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 7; j++) {
          if (
            puissance4[i][j] === currentPlayer &&
            puissance4[i + 1][j] === currentPlayer &&
            puissance4[i + 2][j] === currentPlayer &&
            puissance4[i + 3][j] === currentPlayer
          ) {
            alert(`Joueur ${currentPlayer} a gagné`)
            return true
          }
        }
      }

      // 3. vérifier les diagonales
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 4; j++) {
          if (
            puissance4[i][j] === currentPlayer &&
            puissance4[i + 1][j + 1] === currentPlayer &&
            puissance4[i + 2][j + 2] === currentPlayer &&
            puissance4[i + 3][j + 3] === currentPlayer
          ) {
            alert(`Joueur ${currentPlayer} a gagné`)
            return true
          }
        }
      }

      for (let i = 0; i < 3; i++) {
        for (let j = 3; j < 7; j++) {
          if (
            puissance4[i][j] === currentPlayer &&
            puissance4[i + 1][j - 1] === currentPlayer &&
            puissance4[i + 2][j - 2] === currentPlayer &&
            puissance4[i + 3][j - 3] === currentPlayer
          ) {
            alert(`Joueur ${currentPlayer} a gagné`)
            return true
          }
        }
      }
      return false
    },
    switchPlayer() {
      if (currentPlayer === player1) {
        currentPlayer = player2
      } else {
        currentPlayer = player1
      }
    },
    startGame() {
      this.createPlayground()
    },
    handleReset() {
      for (let i = 0; i < 6; i++) {
        for (let j = 0; j < 7; j++) {
          puissance4[i][j] = 0
        }
      }

      const circles = document.querySelectorAll('.circle')
      circles.forEach((circle) => {
        circle.classList.remove('player1')
        circle.classList.remove('player2')
      })
      currentPlayer = player1
      win = false
    },
  },
})
</script>

<style>
body,
html {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  font-family: 'Roboto', sans-serif;
  font-size: 16px;
  color: #333;
  background: #f5f5f5;
}

#app {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.playground {
  display: flex;
}

.column {
  margin-right: 1px;
  display: flex;
  flex-direction: column;
}

.column:last-child {
  margin-right: 0;
}

.cell {
  background-color: blue;
  padding: 1px;
}

.circle {
  background-color: white;
  width: 50px;
  height: 50px;
  margin: 10px;
  border-radius: 100px;
}

.player1 {
  background-color: red;
}

.player2 {
  background-color: yellow;
}

.win {
  background-color: green;
}
</style>
