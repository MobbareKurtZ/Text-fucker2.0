<template>
  <div id="app">
    <div class="container">
      <h1>TEXT FLOOPER</h1>
      <h3>Floop your text up</h3>
      <div v-if="this.state == 'lang'">
        <select v-model="startLang">
          <option disabled value>Choose language</option>
          <option :value="'sv'">Swedish</option>
          <option :value="'en'">English</option>
        </select>
        <button @click="state = 'text'">Select</button>
      </div>
      <div v-if="this.state == 'text'">
        <section class="settings">
          <textarea placeholder="Text to be flooped..." v-model="text"></textarea>
          <button @click="runTranslations()">Floop</button>
          <h4>Floop level: {{this.level}}</h4>
          <div class="slider-container">
            <input v-model="level" type="range" min="1" max="20" class="slider">
          </div>
          <button :class="{enabled: this.extreme}" @click="extremeMode()" class="extreme">ExXTREME FL00P</button>
        </section>
        <section class="result">
          <h4 v-if="this.curLang">{{this.curLang.name}}</h4>
          <textarea placeholder="Flooped text..." readonly v-model="this.result"></textarea>
          <button v-if="this.done" @click="reRun()">Needs moar floop!</button>
        </section>
        <ul>
          <li
            :class="{disabled: !lang.enabled}"
            v-for="lang in this.langs"
            :key="lang.id"
          >{{lang.name}}</li>
        </ul>
      </div>
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
        {
          enabled: true,
          name: "Afrikaans",
          extreme: false,
          id: "af"
        },
        {
          enabled: true,
          name: "Irish",
          extreme: false,
          id: "ga"
        },
        {
          enabled: true,
          name: "Albanian",
          extreme: false,
          id: "sq"
        },
        {
          enabled: true,
          name: "Italian",
          extreme: false,
          id: "it"
        },
        {
          enabled: true,
          name: "Arabic",
          extreme: true,
          id: "ar"
        },
        {
          enabled: true,
          name: "Japanese",
          extreme: true,
          id: "ja"
        },
        {
          enabled: true,
          name: "Azerbaijani",
          extreme: true,
          id: "az"
        },
        {
          enabled: true,
          name: "Kannada",
          extreme: true,
          id: "kn"
        },
        {
          enabled: true,
          name: "Basque",
          extreme: true,
          id: "eu"
        },
        {
          enabled: true,
          name: "Korean",
          extreme: true,
          id: "ko"
        },
        {
          enabled: true,
          name: "Bengali",
          extreme: true,
          id: "bn"
        },
        {
          enabled: true,
          name: "Latin",
          extreme: false,
          id: "la"
        },
        {
          enabled: true,
          name: "Belarusian",
          extreme: true,
          id: "be"
        },
        {
          enabled: true,
          name: "Latvian",
          extreme: false,
          id: "lv"
        },
        {
          enabled: true,
          name: "Bulgarian",
          extreme: false,
          id: "bg"
        },
        {
          enabled: true,
          name: "Lithuanian",
          extreme: false,
          id: "lt"
        },
        {
          enabled: true,
          name: "Catalan",
          extreme: false,
          id: "ca"
        },
        {
          enabled: true,
          name: "Macedonian",
          extreme: false,
          id: "mk"
        },
        {
          enabled: true,
          name: "Chinese Simplified",
          extreme: true,
          id: "zh-CN"
        },
        {
          enabled: true,
          name: "Chinese Traditional",
          extreme: true,
          id: "zh-TW"
        },
        {
          enabled: true,
          name: "Malay",
          extreme: true,
          id: "ms"
        },
        {
          enabled: true,
          name: "Maltese",
          extreme: false,
          id: "mt"
        },
        {
          enabled: true,
          name: "Croatian",
          extreme: false,
          id: "hr"
        },
        {
          enabled: true,
          name: "Norwegian",
          extreme: false,
          id: "no"
        },
        {
          enabled: true,
          name: "Czech",
          extreme: false,
          id: "cs"
        },
        {
          enabled: true,
          name: "Persian",
          extreme: true,
          id: "fa"
        },
        {
          enabled: true,
          name: "Danish",
          extreme: false,
          id: "da"
        },
        {
          enabled: true,
          name: "Polish",
          extreme: false,
          id: "pl"
        },
        {
          enabled: true,
          name: "Dutch",
          extreme: false,
          id: "nl"
        },
        {
          enabled: true,
          name: "Portuguese",
          extreme: false,
          id: "pt"
        },
        {
          enabled: true,
          name: "English",
          extreme: false,
          id: "en"
        },
        {
          enabled: true,
          name: "Romanian",
          extreme: false,
          id: "ro"
        },
        {
          enabled: true,
          name: "Esperanto",
          extreme: false,
          id: "eo"
        },
        {
          enabled: true,
          name: "Russian",
          extreme: true,
          id: "ru"
        },
        {
          enabled: true,
          name: "Estonian",
          extreme: false,
          id: "et"
        },
        {
          enabled: true,
          name: "Serbian",
          extreme: false,
          id: "sr"
        },
        {
          enabled: true,
          name: "Filipino",
          extreme: true,
          id: "tl"
        },
        {
          enabled: true,
          name: "Slovak",
          extreme: false,
          id: "sk"
        },
        {
          enabled: true,
          name: "Finnish",
          extreme: true,
          id: "fi"
        },
        {
          enabled: true,
          name: "Slovenian",
          extreme: false,
          id: "sl"
        },
        {
          enabled: true,
          name: "French",
          extreme: false,
          id: "fr"
        },
        {
          enabled: true,
          name: "Spanish",
          extreme: false,
          id: "es"
        },
        {
          enabled: true,
          name: "Galician",
          extreme: false,
          id: "gl"
        },
        {
          enabled: true,
          name: "Swahili",
          extreme: true,
          id: "sw"
        },
        {
          enabled: true,
          name: "Georgian",
          extreme: false,
          id: "ka"
        },
        {
          enabled: true,
          name: "Swedish",
          extreme: false,
          id: "sv"
        },
        {
          enabled: true,
          name: "German",
          extreme: false,
          id: "de"
        },
        {
          enabled: true,
          name: "Tamil",
          extreme: true,
          id: "ta"
        },
        {
          enabled: true,
          name: "Greek",
          extreme: true,
          id: "el"
        },
        {
          enabled: true,
          name: "Telugu",
          extreme: true,
          id: "te"
        },
        {
          enabled: true,
          name: "Gujarati",
          extreme: true,
          id: "gu"
        },
        {
          enabled: true,
          name: "Thai",
          extreme: true,
          id: "th"
        },
        {
          enabled: true,
          name: "Haitian Creole",
          extreme: true,
          id: "ht"
        },
        {
          enabled: true,
          name: "Turkish",
          extreme: false,
          id: "tr"
        },
        {
          enabled: true,
          name: "Hebrew",
          extreme: true,
          id: "iw"
        },
        {
          enabled: true,
          name: "Ukrainian",
          extreme: false,
          id: "uk"
        },
        {
          enabled: true,
          name: "Hindi",
          extreme: true,
          id: "hi"
        },
        {
          enabled: true,
          name: "Urdu",
          extreme: true,
          id: "ur"
        },
        {
          enabled: true,
          name: "Hungarian",
          extreme: false,
          id: "hu"
        },
        {
          enabled: true,
          name: "Vietnamese",
          extreme: false,
          id: "vi"
        },
        {
          enabled: true,
          name: "Icelandic",
          extreme: true,
          id: "is"
        },
        {
          enabled: true,
          name: "Welsh",
          extreme: true,
          id: "cy"
        },
        {
          enabled: true,
          name: "Indonesian",
          extreme: false,
          id: "id"
        },
        {
          enabled: true,
          name: "Yiddish",
          extreme: true,
          id: "yi"
        }
      ],
      prevLang: null,
      text: null,
      startLang: "",
      newLang: null,
      result: null,
      level: 10,
      extreme: false,
      usedLangs: [],
      curLang: null,
      done: false
    };
  },
  methods: {
    async runTranslations() {
      this.newLang = this.startLang;
      this.result = this.text;
      for (var i = 0; i < this.level; i++) {
        await this.sleep(2000)
        await this.findLang();
        await this.translate();
      }
      this.finalize();
    },
    reRun() {
      this.done = false
      this.text = this.result;
      this.runTranslations();
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
      let test = "";
      res.data[0].forEach(thing => {
        test += thing[0];
      });
      this.result = test;
    },
    findLang() {
      this.prevLang = this.newLang;
      let langs = this.langs;
      if (this.extreme) {
        langs.filter(lang => {
          return lang.extreme;
        });
      }
      let allowed = false;
      let fet;
      while (allowed == false) {
        fet = langs[Math.floor(Math.random() * langs.length)];
        if (fet.enabled) {
          allowed = true;
        }
      }
      this.usedLangs.push(fet);
      this.curLang = fet;
      this.newLang = fet.id;
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
      this.curLang = "Text successfully flooped!";
      let test = "";
      res.data[0].forEach(thing => {
        test += thing[0];
      });
      this.result = test;
      this.done = true;
    },
    extremeMode() {
      this.extreme = !this.extreme;
      if (this.extreme) {
        this.langs.forEach(lang => {
          if (!lang.extreme) {
            lang.enabled = false;
          }
        });
      } else {
        this.langs.forEach(lang => {
          lang.enabled = true;
        });
      }
    },
    sleep(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/main.scss";
</style>