<template>
  <div id="app">
    <div v-if="this.state == 'lang'">
      <select v-model="startLang">
        <option disabled value>välj ett sprol</option>
        <option :value="'sv'">Svenska</option>
        <option :value="'en'">Engelska</option>
      </select>
      <button @click="state = 'text'">välj sprol</button>
    </div>
    <div v-if="this.state == 'text'">
      <input type="text" v-model="text">
      <a href="#" @click="runTranslations()">test</a>
      <input v-model="level" type="range" min="1" max="20" class="slider" >
      <p>{{this.result}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      state: "lang",
      langs: [
        "af",
        "sq",
        "ar",
        "az",
        "eu",
        "bn",
        "be",
        "bg",
        "ca",
        "zh-CN",
        "zh-TW",
        "hr",
        "cs",
        "da",
        "nl",
        "en",
        "eo",
        "et",
        "tl",
        "fi",
        "fr",
        "gl",
        "ka",
        "de",
        "el",
        "gu",
        "ht",
        "iw",
        "hi",
        "hu",
        "is",
        "id",
        "ga",
        "it",
        "ja",
        "kn",
        "ko",
        "la",
        "lv",
        "lt",
        "mk",
        "ms",
        "mt",
        "no",
        "fa",
        "pl",
        "pt",
        "ro",
        "ru",
        "sr",
        "sk",
        "sl",
        "es",
        "sw",
        "sv",
        "ta",
        "te",
        "th",
        "tr",
        "uk",
        "ur",
        "vi",
        "cy",
        "yi"
      ],
      prevLang: null,
      text: null,
      startLang: "",
      newLang: null,
      result: null,
      level: null
    };
  },
  methods: {
    async runTranslations() {
      this.newLang = this.startLang;
      this.result = this.text;
      for (var i = 0; i < this.level; i++) {
        await this.findLang();
        await this.translate();
      }
      this.finalize();
    },
    async translate() {
      let res = await this.$http.get(
        "https://translate.googleapis.com/translate_a/single?client=gtx&sl=" +
          this.prevLang +
          "&tl=" +
          this.newLang +
          "&dt=t&q=" +
          encodeURI(this.result)
      );
      let test = ""
      res.data[0].forEach(thing => {
        test += thing[0]
      })
      this.result = test;
    },
    findLang() {
      this.prevLang = this.newLang;
      this.newLang = this.langs[Math.floor(Math.random() * this.langs.length)];
    },
    async finalize() {
      let res = await this.$http.get(
        "https://translate.googleapis.com/translate_a/single?client=gtx&sl=" +
          this.newLang +
          "&tl=" +
          "sv" +
          "&dt=t&q=" +
          encodeURI(this.result)
      );
      let test = ""
      res.data[0].forEach(thing => {
        test += thing[0]
      })
      this.result = test
    }
  }
};
</script>

<style lang="scss">
  .slider {
    width: 100px;
  }
</style>