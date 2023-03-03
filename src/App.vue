<script>
import BattleLog from './components/BattleLog.vue'
import ControlPanel from './components/ControlPanel.vue'
import HealthBar from './components/HealtBar.vue'
export default {
  components: {
    HealthBar,
    BattleLog,
    ControlPanel
  },
  data() {
    return {
      yourHealth: 250,
      yourCurrentHealth: 250,
      yourPower: 20,
      enemyHealth: 200,
      enemyCurrentHealth: 200,
      monsterPower: 15,
      logs: []
    }
  },
  methods: {
    newGame() {
      this.yourCurrentHealth = this.yourHealth
      this.enemyCurrentHealth = this.enemyHealth
      this.logs = []
    },
    surrender() {
      this.yourCurrentHealth = 0
      this.updateLog({
        executor: 'Player',
        action: 'surrender',
        actionValue: ''
      })
    },
    updateLog(log) {
      this.logs.push(log)
    },
    updateAfterAction(value, log) {
      this.updateLog(log)
      log.executor === 'Monster' || log.action === 'heal'
        ? (this.yourCurrentHealth = value)
        : (this.enemyCurrentHealth = value)
    }
  }
}
</script>

<template>
  <header class="p-4 text-center fw-bold fs-1 bg-secondary text-light">Monster Slayer</header>
  <div class="game">
    <div class="game__container">
      <HealthBar owner="Monster" :completeHp="enemyHealth" :currentHp="enemyCurrentHealth" />
      <HealthBar owner="Player" :completeHp="yourHealth" :currentHp="yourCurrentHealth" />
      <ControlPanel
        :enemyCurrentHealth="enemyCurrentHealth"
        :monsterPower="monsterPower"
        :yourCurrentHealth="yourCurrentHealth"
        :yourPower="yourPower"
        @fight="updateAfterAction"
        @surrender="surrender"
        @newGame="newGame"
      />
      <BattleLog :logs="logs" :yourCurrentHealth="yourCurrentHealth" />
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
}
</style>
