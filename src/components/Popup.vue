<template>
  <div class="popup-container" id="popup-container" v-if="finalMessage">
    <div class="popup">
      <h2 id="final-message">{{ finalMessage }}</h2>
      <h3 id="final-message-reveal-word" v-show="status === 'lose'">
        ... the word was: {{ word }}
      </h3>
      <button id="play-button" @click="reset">Play Again</button>
    </div>
  </div>
</template>

<script>
import { computed } from "vue";
export default {
  props: {
    status: {
      type: String,
      default: "",
    },
    word: {
      type: String,
      default: "",
    },
  },
  setup(props, { emit }) {
    const finalMessage = computed(() => {
      if (props.status === "win") return "Congratulations! You won! 😃";
      if (props.status === "lose") return "Unfortunately you lost. 😕";
      return "";
    });

    const reset = () => emit("reset");

    return { finalMessage, reset };
  },
};
</script>
