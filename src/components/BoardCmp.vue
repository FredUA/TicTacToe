<template>
  <section class="board">
    <ul v-for="(row, r) of 3" :key="r" class="row">
      <li
        v-for="(cell, c) of 3"
        :key="c"
        @click="move($event, { r, c })"
        class="cell"
      ></li>
    </ul>
    <Transition name="slide-fade">
      <div v-if="hasWinner || draw" class="action-wrapper">
        <p v-if="hasWinner && !draw">The {{ winner }} has WON!</p>
        <p v-else>It is a DRAW!!!</p>
        <button @click="$emit('resetCmp', 1)">Restart</button>
      </div>
    </Transition>
  </section>
</template>

<script>
import { reactive, ref } from 'vue';

export default {
  name: 'BoardCmp',
  setup() {
    const cells = reactive([
      [[], [], []],
      [[], [], []],
      [[], [], []],
    ]);
    let side = true;
    let movements = 0;
    let hasWinner = ref(false);
    let winner = ref(null);
    let draw = ref(false);

    const reset = () => {
      side = true;
      movements = 0;
      hasWinner.value = false;
    };

    const move = (e, key) => {
      cells[key.r][key.c] = side;
      if (
        e.target.classList.contains('⭕️') ||
        e.target.classList.contains('❌') ||
        hasWinner.value
      ) {
        return;
      }

      if (side) {
        e.target.classList.add('⭕️');
      } else {
        e.target.classList.add('❌');
      }

      side = !side;
      ++movements;

      const theEnd = (isDraw, _el) => {
        alert(isDraw, _el);
        if (!isDraw) {
          hasWinner.value = true;
          winner.value = _el ? '⭕️' : '❌';
        } else {
          draw.value = true;
        }
        navigator.vibrate([500, 100, 50, 100, 50]);
      };

      if (movements >= 5) {
        if (
          cells[key.r][0] === cells[key.r][1] &&
          cells[key.r][0] === cells[key.r][2]
        ) {
          theEnd(false, cells[key.r][0]);
        } else if (
          cells[0][key.c] === cells[1][key.c] &&
          cells[0][key.c] === cells[2][key.c]
        ) {
          theEnd(false, cells[0][key.c]);
        } else if (
          (cells[0][0] === cells[1][1] && cells[0][0] === cells[2][2]) ||
          (cells[0][2] === cells[1][1] && cells[0][2] === cells[2][0])
        ) {
          theEnd(false, cells[1][1]);
        } else if (movements === 9) {
          theEnd(true);
        }
      }
    };

    return {
      cells,
      move,
      hasWinner,
      winner,
      draw,
    };
  },
};
</script>

<style>
.board {
  aspect-ratio: 1 / 1;
  width: min(65vw, 75vh);
  position: relative;
}
.row {
  display: flex;
  padding: 0;
  margin: 0;
  height: calc(100% / 3);
}
.row:not(:last-of-type) {
  border-bottom: 3px solid purple;
}
.cell {
  width: calc(100% / 3);
  height: 100%;
  cursor: pointer;
  list-style: none;
  display: grid;
  place-items: center;
  font-size: var(--step-0);
  -webkit-tap-highlight-color: transparent;
}
.cell:not(:last-child) {
  border-right: 3px solid purple;
}
.❌:after {
  content: '❌';
  filter: drop-shadow(2px 4px 6px black);
}
.⭕️:after {
  content: '⭕️';
  filter: hue-rotate(225deg) drop-shadow(2px 4px 6px black);
}
.action-wrapper {
  text-align: center;
  position: absolute;
  inset: 100% 0 auto 0;
  font-size: max(calc(0.92 * 2vh), calc(0.92 * 0.5vw));
  color: #0074ca;
}
.action-wrapper button {
  border: none;
  background-color: #0074ca;
  padding: 10px 20px;
  color: white;
  border-radius: 20px;
  font-size: inherit;
  font-variant-caps: unicase;
}
/*
  Enter and leave animations can use different
  durations and timing functions.
*/
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(20px);
  opacity: 0;
}
</style>
