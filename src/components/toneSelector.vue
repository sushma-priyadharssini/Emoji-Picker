<template>
  <div class="home">
    <div
      class="emoji selected-emoji"
      v-html="displayedEmoji"
      @click="emojiSelector(displayedEmoji)"
    />
    <transition name="fade">
      <div v-if="showTones" class="emoji-tones">
        <div
          class="emoji emoji-tone"
          v-for="tone in Object.values(skinTones).filter(tone => tone != this.displayedTone)"
          :key="tone"
          @click="selectTone(tone)"
          v-html="mergeTypeAndTone(displayedEmoji, tone)"
        />
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "toneSelector",
  props: {
    selectedEmoji: String
  },
  data() {
    return {
      showTones: false,
      displayedTone: "",
      displayedEmoji: this.selectedEmoji,
      skinTones: {
        default: "",
        light: "&#x1F3FB;",
        mediumLight: "&#x1F3FC;",
        medium: "&#x1F3FD;",
        mediumDark: "&#x1F3FE;",
        dark: "&#x1F3FF;"
      }
    };
  },
  methods: {
    mergeTypeAndTone: function(type, tone) {
      return type.substr(0, type.indexOf(";") + 1) + tone;
    },
    emojiSelector: function(type) {
      this.showTones = true;

      this.selectTone = function(tone) {
        this.displayedEmoji = this.mergeTypeAndTone(type, tone);
        this.displayedTone = tone;
        this.showTones = false;
      };
    }
  }
};
</script>
<style scoped >
.emoji {
  padding-top: 20px;
  font-size: 90px;
  cursor: pointer;
}

.emoji-tones {
  display: flex;
  margin: 0 20px;
  border-left: 2px solid rgb(144, 144, 144, 0.3);
  padding-left: 10px;
  line-height: 60px;
}

.emoji-tone {
  margin: 0 5px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.home {
  display: flex;
  align-items: center;
  padding: 30px;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
