<script setup lang="ts">
import { useStore, GameStatus } from "@/stores/store";
import Toast from "@/components/Toast/Toast.vue";

const { state } = useStore();
</script>

<template>
  <div
    class="gameGrid"
    :class="{ 'gameGrid--init': state.status === GameStatus.INIT }"
    :style="`--columnsCount: ${state.wordLength}; --rowsCount: ${state.rowsCount}`"
  >
    <div class="gameGrid__grid">
      <div
        v-for="(_, rowIndex) in state.rowsCount"
        :key="rowIndex"
        class="gameGrid__row"
      >
        <div
          v-for="(_, colIndex) in state.wordLength"
          :key="colIndex"
          :class="`gameGrid__cell ${
            state.guesses[rowIndex]?.[colIndex]?.status
              ? 'gameGrid__cell--' + state.guesses[rowIndex][colIndex].status
              : ''
          }`"
        >
          <template v-if="state.guesses[rowIndex]?.[colIndex]">
            {{ state.guesses[rowIndex][colIndex].letter }}
          </template>
        </div>

        <transition name="fade">
          <Toast
            v-if="
              state.status === GameStatus.GUESS_NOT_EXIST &&
              rowIndex === state.activeRow
            "
          >
            Такого слова нет в базе
          </Toast>
        </transition>
      </div>
      <div v-if="state.status === GameStatus.FAILURE" class="gameGrid__failure">
        {{ state.secretWord }}
      </div>
    </div>
  </div>
</template>

<style src="./GameGrid.scss" lang="scss"></style>
