<template>
  <Header />
  <div class="game-container">
    <Figure :wrong-count="wrongLetters.length" />
    <WrongLetters :wrong-letters="wrongLetters" />
    <Word :letters="letters" :correct-letters="correctLetters" />
  </div>
  <Popup :status="status" :word="word" @reset="reset" />
  <Notification :show="notification" />
</template>

<script>
import { ref, computed } from "vue";
import "./assets/style.css";
import onkeydown from "./assets/onKeydown";
import Header from "./components/Header";
import Figure from "./components/Figure";
import WrongLetters from "./components/WrongLetters";
import Word from "./components/Word";
import Popup from "./components/Popup";
import Notification from "./components/Notification";

const words = ["Play", "football", "love"];
const randomWord = () => words[Math.floor(Math.random() * words.length)]

export default {
  components: { Header, Figure, Word, WrongLetters, Popup, Notification },
  setup() {
    const word = ref(randomWord());
    const guessedLetters = ref([]);
    const notification = ref(false);

    const letters = computed(() => 
      word.value.split('')
    );

    const wrongLetters = computed(() => 
      guessedLetters.value.filter((l) => !letters.value.includes(l))
    );

    const correctLetters = computed(() => 
      guessedLetters.value.filter((l) => letters.value.includes(l))
    );

    const status = computed(() => {
      if (wrongLetters.value.length === 6) return "lose";
      if (letters.value.every((l) => guessedLetters.value.includes(l)))
        return "win";
      return "";
    });

    const reset = () => {
      guessedLetters.value = [];
      word.value = randomWord();
    };

    const showNotification = () => {
      notification.value = true;
      setTimeout(() => (notification.value = false), 2000);
    };

    onkeydown((event) => {
      const letter = event.key.toLowerCase()
      if (event.keyCode < 65 || event.keyCode > 90) return
      if (status.value) return;
      if (guessedLetters.value.includes(letter)) {
        showNotification();
        return;
      }
      guessedLetters.value.push(letter);
    });

    return {
      letters,
      word,
      wrongLetters,
      correctLetters,
      guessedLetters,
      notification,
      status,
      reset,
    };
  },
};
</script>

