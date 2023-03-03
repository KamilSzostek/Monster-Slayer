<template>
  <div
    @click="fight"
    class="row d-flex justify-content-center"
    v-if="enemyCurrentHealth > 0 && yourCurrentHealth > 0"
  >
    <button id="attack" class="col-md-5 btn btn-warning border border-1 border-dark">attack</button>
    <button
      id="special-attack"
      v-if="attackCounter > 2"
      class="col-md-5 btn btn-danger border border-1 border-dark"
    >
      specialAttack
    </button>
    <button v-else class="col-md-5 btn btn-danger border border-1 border-outline-dark disabled">
      specialAttack
    </button>
    <button id="heal" class="col-md-5 btn btn-success border border-1 border-dark">heal</button>
    <button id="surrender" class="col-md-5 btn btn-secondary border border-1 border-dark">
      surrender
    </button>
  </div>
  <div class="text-center" v-else>
    <h3>Game over!</h3>
    <p class="fs-3 text-light">
      <span v-if="yourCurrentHealth > 0" class="bg-success p-2"> You win!</span>
      <span v-else class="bg-danger p-2"> You lose!</span>
    </p>
    <button class="btn btn-outline-secondary" @click="newGame">Start new Game</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      attackCounter: 0
    }
  },
  props: {
    enemyCurrentHealth: Number,
    monsterPower: Number,
    yourCurrentHealth: Number,
    yourPower: Number
  },
  methods: {
    monsterAttack() {
      const value = this.yourCurrentHealth - Math.floor(Math.random() * this.monsterPower)

      const log = {
        executor: 'Monster',
        action: 'attacks',
        actionValue: value
      }
      this.$emit('fight', value, log)
    },
    attack() {
      const value = this.enemyCurrentHealth - Math.floor(Math.random() * this.yourPower)
      if (this.attackCounter < 3) this.attackCounter++
      return value
    },
    specialAttack() {
      const value = this.enemyCurrentHealth - Math.floor(Math.random() * this.yourPower * 2)
      this.attackCounter = 0
      const monsterHealthBar = document.body.querySelector('#Monster')
      monsterHealthBar.classList.add('special-attack')
      setTimeout(() => monsterHealthBar.classList.remove('special-attack'), 500)
      return value
    },
    heal() {
      const value = this.yourCurrentHealth + Math.floor(Math.random() * this.yourPower)
      return value
    },
    newGame() {
      this.$emit('newGame')
    },
    fight(e) {
      let value = 0
      switch (e.target.id) {
        case 'attack':
          value = this.attack()
          break
        case 'special-attack':
          value = this.specialAttack()
          break
        case 'heal':
          value = this.heal()
          break
        case 'surrender':
          this.$emit('surrender')
          return
        default:
          return
      }
      const log = {
        executor: 'Player',
        action: e.target.textContent,
        actionValue: value
      }
      this.$emit('fight', value, log)
      if (this.yourCurrentHealth > 0)
        setTimeout(() => {
          this.monsterAttack()
        }, 250)
    }
  }
}
</script>

<style lang="scss" scoped>
div {
  gap: 10px;
  button {
    text-transform: capitalize;
    font-size: 2rem;
  }
}
</style>
