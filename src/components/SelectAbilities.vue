<template>
  <div id="select-abilities" class="main-col-content">
    <div class="content">
      <h2 class="section-header">Время билдиться!</h2>
      <p>
        {{ name }} имеет шесть показателей характеристик, которые представляют
        {{ pronouns.their }} самые основные атрибуты. Эти показатели
        способностей отражают необузданный талант и мастерство {{ name }}. Эти
        оценки и модификаторы, которые {{ pronouns.their }} создают, влияют
        почти на все аспекты его навыков и способностей.
      </p>

      <!-- <h3>Point Buy</h3>
            <p>
                In Pathfinder, there are many ways to calculate your character's starting ability scores. 
                But the most popular is Point Buy because it is simple and more fair than any other method. 
                This is the method we will be using here.</p>
            <p>
                You will receive a number of points to spend on increasing your character's basic ability scores. 
                All ability scores start at a base value of 10. 
                You can increase an ability score by spending some of your points and likewise gain more points by decreasing an a score. 
                No score can be reduced below 7 or raised above 18.
            </p> -->
    </div>
    <!-- <div class="content box-shadow text-center">
            <h4 class="ntm">Select a campaign type.</h4>
            <div class="radio-toolbar point-limits content">
				<input v-model="pointLimit" type="radio" name="pointLimit" id="select-point-low" value="10" checked>
				<label for="select-point-low">
                    <div class="point-limit">
                        Low Fantasy<br>
                        <strong>10 Points</strong>
                    </div>
                </label>
				<input v-model="pointLimit" type="radio" name="pointLimit" id="select-point-standard" value="15">
				<label for="select-point-standard">
                    <div class="point-limit">
                        Standard Fantasy<br>
                        <strong>15 Points</strong>
                    </div>
                </label>
				<input v-model="pointLimit" type="radio" name="pointLimit" id="select-point-high" value="20">
				<label for="select-point-high">
                    <div class="point-limit">
                        High Fantasy<br>
                        <strong>20 Points</strong>
                    </div>
                </label>
				<input v-model="pointLimit" type="radio" name="pointLimit" id="select-point-epic" value="25">
				<label for="select-point-epic">
                    <div class="point-limit">
                        Epic Fantasy<br>
                        <strong>25 Points</strong>
                    </div>
                </label>
			</div>
            <p>
                The campaign type determines the number of points you can spend on {{name}}'s ability scores. 
                Check with your game master to see what campaign type he or she is using. 
                A standard campaign lets you spend 15 points.
            </p>
        </div> -->

    <!-- <div class="content text-center">
            <h3 :class="{'text-danger': pointsSpent > pointLimit}">Points Spent {{pointsSpent}} / {{pointLimit}}</h3>
        </div> -->

    <div class="content box-shadow text-center">
      <div class="row">
        <table class="ability-controls col-lg-6 col-md-6 col-sm-12 col-xs-12">
          <tr class="ability-controls__headings text-center">
            <th>Название</th>
            <th>Характеристика</th>
            <th>Мод</th>
            <th>Буст</th>
          </tr>
          <tr
            v-for="(score, ability) of abilities"
            :key="ability"
            @mouseover="abilityHover = ability"
            class="ability-control text-center"
          >
            <td class="ability-control__ability text-left">
              <h4 class="ability-control__label">{{ ability }}</h4>
              <div class="ability-control__bar-back">
                <div
                  class="ability-control__bar-front"
                  :style="{ width: (score / 18) * 100 + '%' }"
                ></div>
              </div>
            </td>
            <td class="">{{ score }}</td>
            <td class="">{{ getMod(score) }}</td>
            <td class="">{{ boosts[ability] }}</td>
            <td>
              <button
                class="btn btn-primary"
                @click="decreaseAbility(ability)"
                :disabled="boosts[ability] == 0"
              >
                -
              </button>
              <button
                class="btn btn-primary"
                @click="increaseAbility(ability)"
                :disabled="boosts[ability] == 2 || NumberBoost == 4"
              >
                +
              </button>
            </td>
          </tr>
        </table>
        <div class="ability-descriptions col-lg-6 col-md-6 hidden-sm hidden-xs">
          <strength-description
            v-if="abilityHover == 'сила'"
          ></strength-description>
          <dexterity-description
            v-if="abilityHover == 'ловкость'"
          ></dexterity-description>
          <constitution-description
            v-if="abilityHover == 'телосложение'"
          ></constitution-description>
          <intelligence-description
            v-if="abilityHover == 'интеллект'"
          ></intelligence-description>
          <wisdom-description
            v-if="abilityHover == 'мудрость'"
          ></wisdom-description>
          <charisma-description
            v-if="abilityHover == 'харизма'"
          ></charisma-description>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import StrengthDescription from "./ability-descriptions/StrengthDescription";
import DexterityDescription from "./ability-descriptions/DexterityDescription";
import ConstitutionDescription from "./ability-descriptions/ConstitutionDescription";
import IntelligenceDescription from "./ability-descriptions/IntelligenceDescription";
import WisdomDescription from "./ability-descriptions/WisdomDescription";
import CharismaDescription from "./ability-descriptions/CharismaDescription";

export default {
  data: function () {
 
    return {
      
      // BaseBoost: {
      //   сила: 0,
      //   ловкость: 0,
      //   телосложение: 0,
      //   интеллект: 0,
      //   мудрость: 0,
      //   харизма: 0,
      // },

      
      NumberBoost: 0,
      pointLimit: 100,
      abilityHover: "",
    };
  },
  props: {
    abilities: Object,
    name: String,
    pronouns: Object,
    boosts: Object,

  },
  components: {
    StrengthDescription,
    DexterityDescription,
    ConstitutionDescription,
    IntelligenceDescription,
    WisdomDescription,
    CharismaDescription,
  },
  computed: {
    pointsSpent: function () {
      var pointsSpent = 0;
      var abilityScores = Object.values(this.abilities);
      for (var score of abilityScores) {
        pointsSpent += this.getCost(score);
      }
      return pointsSpent;
    },
    // boosts: function () {
    //   return {
    //     сила: 0,
    //     ловкость: 0,
    //     телосложение: 0,
    //     интеллект: 0,
    //     мудрость: 0,
    //     харизма: 0,
    //   };
      
    // },
  },
  methods: {
    //Boost
    getCost: function (ability) {
      return 0;
    },
    getBoost: function (ability) {
      var ScoreBoosts = Object.values(this.boosts);
      return ScoreBoosts[ability];
    },
    getMod: function (score) {
      var bonus = Math.floor((score - 10) / 2);
      if (bonus > 0) {
        return "+" + bonus;
      } else {
        return bonus;
      }
    },
    increaseAbility: function (ability) {
      if (this.abilities[ability] < 18) {
        this.abilities[ability]++;
        this.abilities[ability]++;
        this.boosts[ability] = 2;
        // this.BaseBoost[ability] = 2;
        this.NumberBoost++;
      }
      this.$emit("set-abilities", this.abilities);
      this.$emit(
        "set-ability-points-ratio",
        this.pointsSpent / this.pointLimit
      );
    },
    decreaseAbility: function (ability) {
      if (this.abilities[ability] < 18 && this.boosts[ability] == 2) {
        this.abilities[ability]--;
        this.abilities[ability]--;
        this.boosts[ability] = 0;
        // this.BaseBoost[ability] = 0;
        this.NumberBoost--;
      }
      this.$emit("set-abilities", this.abilities);
      this.$emit(
        "set-ability-points-ratio",
        this.pointsSpent / this.pointLimit
      );
    },
  },
};
</script>

<style lang="scss">
.point-limits {
  display: flex;
  align-items: stretch;
  flex-wrap: wrap;

  label {
    font-weight: normal;
    flex: 1 1 23%;

    @media only screen and (max-width: 767px) {
      flex: 1 1 48%;
    }

    & > * {
      top: 50%;
      position: relative;
      transform: translateY(-50%);
    }
  }
}

.ability-controls {
  &__headings {
    th {
      text-align: center;
    }
  }

  .ability-control {
    &:hover {
      background-color: var(--secondary-color-light);
    }

    td {
      padding: 2rem 0.5rem;
    }

    &__label {
      margin-top: 0;
      text-transform: capitalize;

      h4 {
        margin-top: 0;
      }
    }

    &__ability {
      text-align: left;
    }

    &__bar-back {
      width: 90%;
      height: 8px;
      border-radius: 4px;
      background: rgba(0, 0, 0, 0.06);
      box-shadow: inset 2px -2px 2px 2px rgba(0, 0, 0, 0.12);
      margin-top: 10px;
    }

    &__bar-front {
      background-color: var(--light-color);
      height: 100%;
      box-shadow: inset -2px 2px 2px 2px rgba(0, 0, 0, 0.06);
      border-radius: 4px;
      transition: width 300ms linear;
    }
  }
}

.ability-descriptions {
  p {
    text-align: left;
  }

  ul {
    text-align: left;
    list-style-type: disc !important;
    padding-left: 1rem;

    li {
      font-size: 1.5rem;
    }
  }
}
</style>