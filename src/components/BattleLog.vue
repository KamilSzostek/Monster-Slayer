<template>
  <div class="battle-log">
    <h3 class="p-5 fs-2 fw-bold">Battle Log</h3>
    <ul>
      <li v-for="{ executor, action, actionValue } in battleLogs" key="Math.random()">
        <span :class="executor === 'Player' ? 'you' : 'monster'">{{ `${executor} ` }}</span
        >{{ action }} {{ yourCurrentHealth > 0 ? 'and deals' : '' }}
        <span class="value">{{ actionValue }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    logs: [],
    yourCurrentHealth: String
  },
  computed: {
    battleLogs() {
      return this.logs
    }
  }
}
</script>
<style lang="scss" scoped>
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
</style>
