<template>
  <div id="app">
    <div class="header">
      <img
        :src="'src/assets/pathfinder-logo.png'"
        style="max-height: 80px; width: auto"
      />
      <!-- <div class="container-right">
    <div class="card">
      <input
        @change="toggleTheme"
        id="checkbox"
        type="checkbox"
        class="switch-checkbox"
      />
      <label for="checkbox" class="switch-label">
  <span>🌙</span>
  <span>☀️</span>
  <div
    class="switch-toggle"
    :class="{ 'switch-toggle-checked': userTheme === 'dark-theme' }"
  ></div>
      </label>
    </div> 
      <h1>Pathfinder 2E Создание персонажа</h1>

  </div> -->
    </div>
    <div class="container">
      
      <div class="row">
        <div class="preview-column col-sm-12 col-md-3 col-lg-3">
          <character-preview
            :name="name"
            :abilities="abilities"
            :race="race"
            :klass="klass"
            :favouredKlass="favouredKlass"
            :feats="feats"
            :myLanguages="myLanguages"
            :mySkills="skill"
            :level="level"
            :background="background"
          >
          </character-preview>
        </div>
        <div class="col-sm-12 col-md-9 col-lg-9">
          <select-name
            v-if="currStep == 0"
            @set-name="setName"
            @set-gender="setGender"
            @set-pronouns="setPronouns"
          >
          </select-name>
          <select-abilities
            v-if="currStep == 1"
            :name="name"
            :pronouns="pronouns"
            :abilities="baseAbilities"
            :boosts="boosts"
            @set-abilities="setAbilities"
            @set-ability-points-ratio="setAbilityPointsRatio"
          >
          </select-abilities>
          <select-race
            v-if="currStep == 2"
            :propRace="race"
            :abilities="abilities"
            :name="name"
            :pronouns="pronouns"
            @set-race="setRace"
            @set-my-languages="setMyLanguages"
          >
          </select-race>
          <select-class
            v-if="currStep == 3"
            :propKlass="klass"
            :race="race"
            :abilities="abilities"
            :name="name"
            :pronouns="pronouns"
            :level="level"
            @set-klass="setKlass"
            @set-favoured-klass="setFavouredKlass"
          >
          </select-class>

          <select-skills
            v-if="currStep == 4"
           
            :skills="skills"
            :klass="klass"
            :race="race"
            :abilities="abilities"
            :name="name"
            :pronouns="pronouns"
          >
          </select-skills>

          <select-background
            v-if="currStep == 5"
            :klass="klass"
            :race="race"
            :abilities="abilities"
            :name="name"
            :pronouns="pronouns"
            :propbackground="background"
            @set-background="setBackground"
            
          >
          </select-background>

          <!-- <select-feat
            v-if="currStep == 5"
            :klass="klass"
            :race="race"
            :abilities="abilities"
            :name="name"
            :pronouns="pronouns"
          >
          </select-feat> -->

          <!-- <select-equipment
            v-if="currStep == 6"
            :feats="feats"
            :klass="klass"
            :race="race"
            :abilities="abilities"
            :name="name"
            :pronouns="pronouns"
          >
          </select-equipment> -->

          <select-details
            v-if="currStep == 6"
            :feats="feats"
            :klass="klass"
            :race="race"
            :abilities="abilities"
            :name="name"
            :pronouns="pronouns"
          >
          </select-details>

          <div class="page-buttons">
            <button
              v-if="currStep > 0"
              v-on:click="prevStep"
              class="btn btn-lg btn-primary btn-previous"
            >
              Назад
            </button>
            <div>
              <button
                v-if="currStep < steps.length - 1"
                v-on:click="nextStep"
                class="btn btn-lg btn-primary btn-next"
                :disabled="!canProceed"
              >
                Вперёд
                <p id="proceedError">
                  {{ proceedError }}
                </p>
              </button>
              <button
                v-if="currStep == steps.length - 1"
                class="btn btn-lg btn-primary btn-next"
                disabled="true"
              >
                Заканчиваем!
                <p id="proceedError">"Функционал ещё не готов"</p>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
var character = {
  debug: true,
  name: "",
  setMessageAction(newValue) {
    if (this.debug) console.log("setMessageAction triggered with", newValue);
    this.state.message = newValue;
  },
  clearMessageAction() {
    if (this.debug) console.log("clearMessageAction triggered");
    this.state.message = "";
  },
};

import CharacterPreview from "./components/CharacterPreview.vue";
import SelectName from "./components/SelectName.vue";
import SelectAbilities from "./components/SelectAbilities.vue";
import SelectRace from "./components/SelectRace.vue";
import SelectClass from "./components/SelectClass.vue";
import SelectSkills from "./components/SelectSkills.vue";
import SelectFeat from "./components/SelectFeat.vue";
import SelectEquipment from "./components/SelectEquipment.vue";
import SelectDetails from "./components/SelectDetails.vue";
import SelectBackground from "./components/SelectBackground.vue";

// import skillData from "../data/skills.json";
export default {
  mounted() {
    const initUserTheme = this.getTheme() || this.getMediaPreference();
  this.setTheme(initUserTheme);
  },
  name: "app",
  components: {
    SelectName,
    CharacterPreview,
    SelectAbilities,
    SelectRace,
    SelectClass,
    SelectSkills,
    SelectFeat,
    SelectEquipment,
    SelectDetails,
    SelectBackground
  },
  data: function () {
    return {
      //Progress Tracker
      userTheme: "light-theme",
      steps: [
        "Name",
        "Abilities",
        "Race",
        "Class",
        "Skills",
        "Background",
        "Details",
      ],
      currStep: 0,
      proceedError: "",

      //Name & Gender
      name: "",
      gender: "",
      background: null,
      pronouns: {},
      boosts:  {

        сила: 0,
        ловкость: 0,
        телосложение: 0,
        интеллект: 0,
        мудрость: 0,
        харизма: 0,
      },
    

      level: 1,
      //Abilities
      baseAbilities: {
        сила: 10,
        ловкость: 10,
        телосложение: 10,
        интеллект: 10,
        мудрость: 10,
        харизма: 10,
      },
      abilityPointsRatio: 0,
      skills: {
          Акробатика: 0,
          Аркана: 0,
          Атлетика: 0,
          Ремесло: 0,
          Обман: 0,
          Дипломатия: 0,
          Запугивание: 0,
          Медицина: 0,
          Природа: 0,
          Оккультизм: 0,
          Выступление: 0,
          Религия: 0,
          Общество: 0,
          Скрытность: 0,
          Выживание: 0,
          Воровство: 0,
      },
      //Race
      race: null,
      myLanguages: ["Всеобщий"],
      // mySkills: null,
      NumberSkill: 0,
      //Class
      klass: null,
      favouredKlass: "",
      //Skills
      baseSkills: {
          Акробатика: 0,
          Аркана: 0,
          Атлетика: 0,
          Ремесло: 0,
          Обман: 0,
          Дипломатия: 0,
          Запугивание: 0,
          Медицина: 0,
          Природа: 0,
          Оккультизм: 0,
          Выступление: 0,
          Религия: 0,
          Общество: 0,
          Скрытность: 0,
          Выживание: 0,
          Воровство: 0,
      },
      
      //Feats
      feats: [""],

      //Equipment
      equipment: null,

      //Details
      details: null,
    };
  },
  methods: {
    setTheme(theme) {
    localStorage.setItem("user-theme", theme);
    this.userTheme = theme;
    document.documentElement.className = theme;
  },
  toggleTheme() {
  const activeTheme = localStorage.getItem("user-theme");
  if (activeTheme === "light-theme") {
    this.setTheme("dark-theme");
  } else {
    this.setTheme("light-theme");
  }
},
getMediaPreference() {
  const hasDarkPreference = window.matchMedia(
    "(prefers-color-scheme: dark)"
  ).matches;
  if (hasDarkPreference) {
    return "dark-theme";
  } else {
    return "light-theme";
  }
},
getTheme() {
  return localStorage.getItem("user-theme");
},
    prevStep: function () {
      if (this.currStep > 0) {
        this.currStep--;
        window.scrollTo(0, 240);
      }
    },
    nextStep: function () {
      if (this.currStep < this.steps.length - 1) {
        this.currStep++;
        window.scrollTo(0, 240);
      }
    },
    setName: function (value) {
      this.name = value;
    },
    setGender: function (value) {
      this.gender = value;
    },
    setPronouns: function (value) {
      this.pronouns = value;
    },
    setAbilities: function (value) {
      this.baseAbilities = value;
    },
    setAbilityPointsRatio: function (value) {
      this.abilityPointsRatio = value;
    },
    setRace: function (value) {
      this.race = value;
    },
    setKlass: function (value) {
      this.klass = value;
    },
    setFavouredKlass: function (value) {
      this.favouredKlass = value;
    },
    setMyLanguages: function (value) {
      this.myLanguages = value;
    },
    setMySkills: function (value) {
      this.skills = value;
    },
    setBackground: function (value) {
      this.background = value;
    },
    getMod: function (score) {
      var bonus = Math.floor((score - 10) / 2);
      if (bonus > 0) {
        return "+" + bonus;
      } else {
        return bonus;
      }
    },
  },

  computed: {
    canProceed: function () {
      var result;
      switch (this.currStep) {
        case 0:
          result = this.name && this.name.length > 0;
          this.proceedError = result ? "" : "Выбирите Имя!";
          return result;
        case 1:
          result = this.abilityPointsRatio <= 1;
          this.proceedError = result
            ? ""
            : "!";
          return result;
        case 2:
          result = this.race != null;
          this.proceedError = result ? "" : "Вы должны выбрать Расу!";
          return result;
        case 3:
          result = this.klass != null;
          this.proceedError = result ? "" : "вы должны выбрать класс!";
          return result;
        default:
          return true;
      }
    },
    abilities: function () {
      if (this.race && this.race.abilities ) {


        var str = 0; 
        var dex = 0; 
        var con = 0; 
        var wis = 0; 
        var cha = 0; 
        var int = 0; 
        
        if(this.background && this.background.abilities)
        {
          
          str = str + this.background.abilities.сила;
          dex = dex + this.background.abilities.ловкость;
          con = con + this.background.abilities.телосложение;
          wis = wis + this.background.abilities.мудрость;
          cha = cha + this.background.abilities.харизма;
          int = int + this.background.abilities.интеллект;
        }

        if(this.klass && this.klass.classAbilities)
        {
          if(this.klass.classAbilities.сила && this.klass.classAbilities.сила != 0 ) str = str + this.klass.classAbilities.сила;
          if(this.klass.classAbilities.ловкость && this.klass.classAbilities.ловкость != 0 ) dex = dex + this.klass.classAbilities.ловкость;
          if(this.klass.classAbilities.телосложение && this.klass.classAbilities.телосложение != 0 ) con = con + this.klass.classAbilities.телосложение;
          if(this.klass.classAbilities.мудрость && this.klass.classAbilities.мудрость != 0 ) wis = wis + this.klass.classAbilities.мудрость;
          if(this.klass.classAbilities.харизма && this.klass.classAbilities.харизма != 0 ) cha = cha + this.klass.classAbilities.харизма;
          if(this.klass.classAbilities.интеллект && this.klass.classAbilities.интеллект != 0 ) int = int + this.klass.classAbilities.интеллект;
        }

        return {
          сила: this.baseAbilities.сила + this.race.abilities.сила + str,
          ловкость:
            this.baseAbilities.ловкость + this.race.abilities.ловкость + dex,
          телосложение:
            this.baseAbilities.телосложение + this.race.abilities.телосложение + con,
          интеллект:
            this.baseAbilities.интеллект + this.race.abilities.интеллект + int,
          мудрость: this.baseAbilities.мудрость + this.race.abilities.мудрость + wis,
          харизма: this.baseAbilities.харизма + this.race.abilities.харизма + cha,
        };
      }
      return {
        сила: this.baseAbilities.сила,
        ловкость: this.baseAbilities.ловкость,
        телосложение: this.baseAbilities.телосложение,
        интеллект: this.baseAbilities.интеллект,
        мудрость: this.baseAbilities.мудрость,
        харизма: this.baseAbilities.харизма,
      };
    },
    skill: function () {
      if (this.skills) {
        return {
          Акробатика: Math.floor( ( this.abilities.ловкость - 10 ) / 2 ) + this.skills.Акробатика,
          Аркана: Math.floor( ( this.abilities.интеллект - 10 ) / 2 )+ this.skills.Аркана,
          Атлетика: Math.floor( ( this.abilities.сила - 10 ) / 2 )+ this.skills.Атлетика,
          Ремесло: Math.floor( ( this.abilities.интеллект - 10 ) / 2 )+ this.skills.Ремесло,
          Обман: Math.floor( ( this.abilities.харизма - 10 ) / 2 )+ this.skills.Обман,
          Дипломатия: Math.floor( ( this.abilities.харизма - 10 ) / 2 )+ this.skills.Дипломатия,
          Запугивание: Math.floor( ( this.abilities.харизма - 10 ) / 2 )+ this.skills.Запугивание,
          Медицина: Math.floor( ( this.abilities.мудрость - 10 ) / 2 )+ this.skills.Медицина,
          Природа: Math.floor( ( this.abilities.мудрость - 10 ) / 2 )+ this.skills.Природа,
          Оккультизм: Math.floor( ( this.abilities.интеллект - 10 ) / 2 )+ this.skills.Оккультизм,
          Выступление: Math.floor( ( this.abilities.харизма - 10 ) / 2 )+ this.skills.Выступление,
          Религия: Math.floor( ( this.abilities.мудрость - 10 ) / 2 )+ this.skills.Религия,
          Общество: Math.floor( ( this.abilities.интеллект - 10 ) / 2 )+ this.skills.Общество,
          Скрытность: Math.floor( ( this.abilities.ловкость - 10 ) / 2 )+ this.skills.Скрытность,
          Выживание: Math.floor( ( this.abilities.мудрость - 10 ) / 2 )+ this.skills.Выживание,
          Воровство: Math.floor( ( this.abilities.ловкость - 10 ) / 2 )+ this.skills.Воровство,

        };
      }
      return {
          Акробатика: Math.floor( ( this.abilities.ловкость - 10 ) / 2 ),
          Аркана: Math.floor( ( this.abilities.интеллект - 10 ) / 2 ),
          Атлетика: Math.floor( ( this.abilities.сила - 10 ) / 2 ),
          Ремесло: Math.floor( ( this.abilities.интеллект - 10 ) / 2 ),
          Обман: Math.floor( ( this.abilities.харизма - 10 ) / 2 ),
          Дипломатия: Math.floor( ( this.abilities.харизма - 10 ) / 2 ),
          Запугивание: Math.floor( ( this.abilities.харизма - 10 ) / 2 ),
          Медицина: Math.floor( ( this.abilities.мудрость - 10 ) / 2 ),
          Природа: Math.floor( ( this.abilities.мудрость - 10 ) / 2 ),
          Оккультизм: Math.floor( ( this.abilities.интеллект - 10 ) / 2 ),
          Выступление: Math.floor( ( this.abilities.харизма - 10 ) / 2 ),
          Религия: Math.floor( ( this.abilities.мудрость - 10 ) / 2 ),
          Общество: Math.floor( ( this.abilities.интеллект - 10 ) / 2 ),
          Скрытность: Math.floor( ( this.abilities.ловкость - 10 ) / 2 ),
          Выживание: Math.floor( ( this.abilities.мудрость - 10 ) / 2 ),
          Воровство: Math.floor( ( this.abilities.ловкость - 10 ) / 2 ),
      };
    }
    // skillPoints: function () {
    //   let skills =
    //     parseInt(this.klass.skillPoints) +
    //     parseInt(this.getMod(this.abilities.интеллект));
    //   skills = "human" === this.race.race ? skills + 1 : skills;
    //   if (skills > 0) {
    //     return "sp" === this.favouredKlass ? skills + 1 : skills;
    //   } else {
    //     return "sp" === this.favouredKlass ? 2 : 1;
    //   }
    // },
    // level: function () {
    //   return parseInt(this.level);
    // },
  },

};
</script>

<style lang="scss">
// import {}from "../src/components/_app.scss";

@import url("https://fonts.googleapis.com/css?family=Amethysta|Titillium+Web&display=swap");


// @import "node_modules/progress-tracker/src/styles/progress-tracker.scss";
// @import '~pretty-checkbox/src/pretty-checkbox.scss';

body {
  --darker-color: #1c0000;
  --dark-color: #3a0000;
  --primary-color: #5e0000;
  --light-color: #861515;
  --lighter-color: #af3737;
  --lightest-color: #ffa5a5;
  --secondary-color: #ffeebd;
  --secondary-color-light: #fff6db;
  --secondary-color-lighter: #fffcf2;
}

#app {
  font-family: "Titillium Web", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--darker-color);
  margin-top: 60px;
}

p {
  font-size: 1.6rem;
}

.header {
  text-align: center;

  .progress-tracker {
    max-width: 800px;
    width: 80%;

    .progress-marker::before {
      padding-bottom: 0;
    }

    .progress-step.is-complete .progress-marker::before {
      background-color: var(--primary-color);
    }

    .progress-step {
      &.is-active {
        .progress-marker::before {
          background-color: var(--light-color);
        }

        .progress-text {
          color: var(--light-color);
        }
      }

      &:last-child {
        .progress-text {
          left: -24px;
          transform: translateX(0);
        }
      }

      &:hover {
        .progress-marker::before {
          background-color: #b6b6b6;
        }
        &.is-active .progress-marker::before {
          background-color: var(--light-color);
        }
        &.is-complete .progress-marker::before {
          background-color: var(--primary-color);
        }
      }

      .progress-text {
        overflow: visible;
        position: relative;
        left: 12px;
        transform: translateX(-50%);
      }

      @media only screen and (max-width: 780px) {
        .progress-text {
          display: none;
        }
      }
    }
  }
}

@media only screen and (min-width: 992px) {
  .preview-column {
    position: -webkit-sticky; /* Safari */
    position: sticky;
    top: 2rem;
  }
}

h1,
h2 {
  font-family: "Amethysta", serif;
  color: var(--primary-color);
}

.section-header {
  margin-top: -2rem;
}

.ntm {
  margin-top: 0;
}

h4 {
  font-weight: bold;
  margin-top: 3rem;
}

h5 {
  font-size: 12px;
  border-top: solid thin;
  border-bottom: solid thin;
  text-transform: uppercase;
  padding: 3px 0 2px;
  margin-bottom: 2px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: list-item;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.box-shadow {
  /*box-shadow: 0 2px 6px #ccc;*/
  box-shadow: 0 0.4rem 1.2rem rgba(0, 0, 0, 0.2);
  border-radius: 4px;
}

.content {
  padding: 2rem;
}

.caption {
  font-size: 13px;
  color: rgb(97, 97, 97);
}

.radio-toolbar,
.checkbox-toolbar {
  input[type="radio"],
  input[type="checkbox"] {
    opacity: 0;
    position: fixed;
    width: 0;

    &:hover + label {
      background-color: var(--secondary-color-light);
      border-color: var(--primary-color);
      color: var(--primary-color);
    }

    &:checked + label {
      background-color: var(--light-color);
      border-color: var(--primary-color) !important;
      color: white;
    }
  }



  label {
    display: inline-block;
    background-color: transparent;
    padding: 1rem 2rem;
    margin-right: 1rem;
    font-family: sans-serif, Arial;
    font-size: 16px;
    color: var(--light-color);
    border: 1px solid var(--light-color);
    border-radius: 4px;
    cursor: pointer;



  }
}

input[type="text"] {
  padding: 0.5rem 1rem;
  margin-right: 0.5rem;
  color: #495057;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.page-buttons {
  padding-top: 2rem;
  padding-bottom: 6rem;
  position: relative;
  display: flex;
  justify-content: space-between;

  .btn-next {
    position: absolute;
    right: 0;
  }
}

.btn-primary,
.btn-primary:visited {
  background-color: var(--light-color);
  border-color: var(--light-color);
}

.btn-primary:hover,
.btn-primary:active,
.btn-primary:focus,
.btn-primary:active:focus {
  background-color: var(--primary-color) !important;
  border-color: var(--primary-color) !important;
  color: white;
}

.btn-outline-primary,
.btn-outline-primary:visited {
  color: var(--light-color);
  border-color: var(--light-color);
  background-color: transparent;
}

.btn-outline-primary:active,
.btn-outline-primary:hover,
.btn-outline-primary:active:hover {
  background-color: var(--secondary-color-light);
  border-color: var(--primary-color);
  color: var(--primary-color);
}

#proceedError {
  position: absolute;
  top: 100%;
  margin-top: 0.5rem;
  right: 0;
  font-size: 1.4rem;
  color: white;
}

button:hover #proceedError {
  color: var(--primary-color);
}

.cap {
  text-transform: capitalize;
}

.csli:not(:last-child):after {
  content: ", ";
}

:root {
  --background-color-primary: #ebebeb;
  --background-color-secondary: #fafafa;
  --accent-color: #cacaca;
  --text-primary-color: #222;
  --element-size: 4rem;
}

/* Define styles for the root window with dark - mode preference */



</style>
