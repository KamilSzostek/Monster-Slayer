<script>
export default {
  data() {
    return {
      yourHealth: 250,
      yourCurrentHealth: 250,
      yourPower: 20,
      enemyHealth: 200,
      enemyCurrentHealth: '200',
      monsterPower: 15,
      attackCounter: 0,
      logs: []
    }
  },
  methods: {
    monsterAttack() {
      const value = this.yourCurrentHealth - Math.floor(Math.random() * this.monsterPower)
      this.yourCurrentHealth = value

      const log = {
        executor: 'Monster',
        action: 'attacks',
        actionValue: value
      }
      this.updateLog(log)
    },
    attack() {
      const value = this.enemyCurrentHealth - Math.floor(Math.random() * this.yourPower)
      this.enemyCurrentHealth = value
      if (this.attackCounter < 3) this.attackCounter++
      return value
    },
    specialAttack() {
      const value = this.enemyCurrentHealth - Math.floor(Math.random() * this.yourPower * 2)
      this.enemyCurrentHealth = value
      this.attackCounter = 0
      this.$refs.monsterHealth.classList.add('special-attack')
      setTimeout(() => this.$refs.monsterHealth.classList.remove('special-attack'), 500)
      return value
    },
    heal() {
      const value = this.yourCurrentHealth + Math.floor(Math.random() * this.yourPower)
      this.yourCurrentHealth = value
      return value
    },
    surrender() {
      this.yourCurrentHealth = 0
      return ''
    },
    newGame() {
      this.yourCurrentHealth = this.yourHealth
      this.enemyCurrentHealth = this.enemyHealth
      this.logs = []
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
          value = this.surrender()
          break
        default:
          return
      }
      const log = {
        executor: 'Player',
        action: e.target.textContent,
        actionValue: value
      }
      this.updateLog(log)
      if (this.yourCurrentHealth > 0)
        setTimeout(() => {
          this.monsterAttack()
        }, 250)
    },
    updateLog(log) {
      this.logs.push(log)
    }
  }
}
</script>

<template>
  <header class="p-4 text-center fw-bold fs-1 bg-secondary text-light">Monster Slayer</header>
  <div class="game">
    <div class="game__container">
      <div class="container pb-2" ref="monsterHealth">
        <h3 class="text-dark fs-2">Monster</h3>
        <div class="health-bar">
          <div
            :style="{ width: `${(enemyCurrentHealth * 100) / enemyHealth}%` }"
            class="current-health"
          ></div>
        </div>
      </div>
      <div class="container pb-2">
        <h3 class="text-dark fs-2">Yours</h3>
        <div class="health-bar">
          <div
            :style="{ width: `${(yourCurrentHealth * 100) / yourHealth}%` }"
            class="current-health"
          ></div>
        </div>
      </div>
      <div
        @click="fight"
        class="row d-flex justify-content-center"
        v-if="enemyCurrentHealth > 0 && yourCurrentHealth > 0"
      >
        <button id="attack" class="col-md-5 btn btn-warning border border-1 border-dark">
          attack
        </button>
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
      <div class="battle-log">
        <h3 class="p-5 fs-2 fw-bold">Battle Log</h3>
        <ul>
          <li v-for="log in logs" key="Math.random()">
            <span :class="log.executor === 'Player' ? 'you' : 'monster'">{{
              `${log.executor} `
            }}</span
            >{{ log.action }} {{ yourCurrentHealth > 0 ? 'and deals' : '' }}
            <span class="value">{{ log.actionValue }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.game {
  display: flex;
  justify-content: center;
}
.game__container {
  display: flex;
  flex-direction: column;
  max-width: 600px;
  min-width: 300px;
  margin-top: 5em;

  .container {
    width: 100%;
    margin-bottom: 2em;
    box-shadow: 1px 1px 10px #3333336b;
    .health-bar {
      width: 100%;
      height: 30px;
      padding-inline: 5px;
      background-color: #eee2e2;

      .current-health {
        height: 100%;
        background-color: #c72020;
      }
    }
  }
  div {
    gap: 10px;
    button {
      text-transform: capitalize;
      font-size: 2rem;
    }
  }
  .battle-log {
    min-height: 300px;
    margin-top: 5em;
    border-radius: 0.5em;
    text-align: center;
    box-shadow: 0 0 10px #3333335d;

    ul {
      list-style: none;
      li {
        font-size: 1.4rem;
      }
      span {
        font-weight: bold;
      }
    }
    .you {
      color: green;
    }
    .monster {
      color: royalblue;
    }
    .value {
      color: greenyellow;
    }
  }
  .special-attack {
    animation: special-attack 0.5s ease-out 1;
  }
  @keyframes special-attack {
    25% {
      transform: translateX(-5%);
    }
    50% {
      transform: translateX(10%);
    }
    75% {
      transform: translateX(-2%);
    }
  }
}
</style>
