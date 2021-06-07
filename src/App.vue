<template>
  <div id="app">
    <div
      v-for="(itemRow, indexRow) of tthCollection"
      :key="indexRow"
      class="tthrow"
    >
      <div
        v-for="(item, indexCol) of itemRow.tthRow"
        :key="indexCol"
        class="tthitem"
      >
        <Toothpick
          :isPicked="item"
          @click.native="mClickedHandle(indexRow, indexCol)"
        ></Toothpick>
      </div>
    </div>
    <div class="status" v-show="!isWin">
      <div class="currentplayer">选手：player{{ currentPlayer }}</div>
      <button class="playerchange" @click="playerChangeHandle">
        下一位选手
      </button>
    </div>
    <div class="winnerbar" v-show="isWin">
      获胜选手是：player{{ currentPlayer }}
    </div>
    <button class="restart" @click="restartHandle">重新开始</button>
  </div>
</template>

<script>
import Toothpick from "./components/Toothpick.vue";

export default {
  name: "App",
  components: { Toothpick },
  data() {
    return {
      tthCollection: [
        {
          tthRow: [false, false, false],
        },
        {
          tthRow: [false, false, false, false, false],
        },
        {
          tthRow: [false, false, false, false, false, false, false],
        },
      ],
      selectedRow: null,
      currentPlayer: 1,
      isWin: false,
    };
  },
  methods: {
    mClickedHandle(row, col) {
      if (!this.isWin) {
        if (this.selectedRow == null || this.selectedRow == row) {
          this.tthCollection[row].tthRow[col] = true;
          this.selectedRow = row;
          if (this.getTotalUnselected() == 0) {
            this.playerChangeHandle();
            this.isWin = true;
          }
        } else {
          console.log("请按规则操作");
        }
      }
      this.$forceUpdate();
    },
    playerChangeHandle() {
      if (!this.isWin) {
        if (this.currentPlayer == 1) {
          this.currentPlayer = 2;
        } else {
          this.currentPlayer = 1;
        }
        this.selectedRow = null;
      }
    },
    getTotalUnselected() {
      let i = 0;
      for (let item of this.tthCollection) {
        for (let sub of item.tthRow) {
          if (!sub) {
            i = i + 1;
          }
        }
      }
      return i;
    },
    restartHandle() {
      for (let num in this.tthCollection) {
        this.tthCollection[num].tthRow.fill(false);
      }
      this.selectedRow = null;
      this.currentPlayer = 1;
      this.isWin = false;
      this.$forceUpdate();
    },
  },
};
</script>

<style>
#app {
}
.tthrow {
  width: 50%;
  margin-top: 10px;
  display: flex;
}
.tthitem {
  margin-left: 10px;
}
</style>
