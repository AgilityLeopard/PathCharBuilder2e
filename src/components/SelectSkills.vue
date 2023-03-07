
<template>
  <div id="select-abilities" class="main-col-content">
    <div class="content">
      <h2 class="section-header"></h2>
      <p></p>
      <p></p>
      <p>Y</p>
    </div>
    <div class="content box-shadow text-center">
      <div class="row">
        <table class="skill-controls">
          <tr class="skill-controls__headings text-center">
            <th class="text-left">Навыки</th>
            <!-- <th>Class Skill</th> -->
            <th>Характеристика</th>
            <th>Модификатор</th>
            <!-- <th>Дополнительно</th> -->
            <th>Тренирован?</th>
            <th>Всего</th>
          </tr>
          <template v-for="(skill, skillName) in skillList">
            <tr class="skill-control text-center" :key="'skill-' + skillName">
              <td class="skill-control__name">
                <div>{{ skillName }}</div>
              </td>
              <!-- <td>
                <div v-if="isKlassSkill(skillName)" style="color: #060">
                  <font-awesome-icon
                    :icon="['far', 'check-circle']"
                    size="2x"
                  />
                </div>
                <div v-else style="color: #ccc">
                  <font-awesome-icon
                    :icon="['far', 'times-circle']"
                    size="2x"
                  />
                </div>
              </td> -->
              <td class="cap">{{ skill.ability.substring(0, 3) }}</td>
              <td>{{ calcBonus(abilities[skill.ability]) }}</td>
              <!-- <td>0</td> -->
              <td>
                <div class="pretty p-switch" style="font-size: 2rem">
                  <input
                    type="checkbox"
                    v-model="skill.trained"
                    :value="skill"
                    @change='updateCalc(skill)'
                    
                  />
                  <!-- <div v-if="checked" style="color:#060;">                
                                <font-awesome-icon  :icon="['far', 'check-circle']" size="2x"/>
                            </div>
                            <div v-else style="color:#ccc;">
                                <font-awesome-icon :icon="['far', 'times-circle']" size="2x"/>
                            </div> -->
                  <!-- <div class="state">
                                    <i class="icon mdi mdi-check"></i>
                                    <label></label>
                                </div> -->
                </div>
              </td>
              <td>
                <b>{{ calcSkill(skill) }}</b>
              </td>
            </tr>
          </template>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import skillData from "../data/skills.json";

export default {
  data: function () {
    return {
      skillList: skillData,
      trainedSkills: [],
      SkillNumber: 0,
    };
  },

  props: {
    skillPoints: 0,
    race: Object,
    klass: Object,
    abilities: Object,
    name: String,
    pronouns: Object,
  },
  computed: {},
  methods: {
    calcBonus: function (score) {
      var bonus = Math.floor((score - 10) / 2);
      if (bonus > 0) {
        return "+" + bonus;
      } else {
        return bonus;
      }
    },
    calcSkill: function (skill) {
      let abilityBonus = this.calcBonus(this.abilities[skill.ability]);
      let raceBonus = 0;
      let klassBonus = 0;
      let klassSkill = this.isKlassSkill(skill.name);
      let trained = this.trainedSkills.indexOf(skill) > -1;


      let bonus = Number(abilityBonus);
      if(skill.trained == true)
      {
        bonus = bonus + 3;
        // NumberSkill++;
      }
    
      // NumberSkill--;


      return bonus;
    },
    isKlassSkill: function (skillName) {
      return this.klass.classSkills.includes(skillName);
    },
    updateCalc: function (skill) {
      let abilityBonus = this.calcBonus(this.abilities[skill.ability]);

      let bonus = Number(abilityBonus);

      if(skill.trained == true)
      {
        bonus = bonus + 3;
        // NumberSkill++;
      }
     

      
      console.log(skill.trained);
      
   
      return bonus;
    },
  },
};
</script>

<style lang="scss">
th {
  text-align: center;
}

.skill-controls {
  width: 100%;
}

.skill-control {
  & td {
    padding: 0.5rem 0;
  }

  &__name {
    text-align: left;
    font-size: 1.8rem;

    div {
      margin-left: 1rem;
    }
  }

  &:nth-child(even) {
    background: #eee;
  }
}

.pretty .state .label {
  &:before,
  &:after {
    top: calc(50%);
    transform: translateY(-50%);
  }
}

.pretty input:checked ~ .state label:after {
  background-color: var(--light-color);
}
</style>