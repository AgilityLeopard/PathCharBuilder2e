
<template>
  <div id="select-abilities" class="main-col-content">
    <div class="content">
      <h2 class="section-header"></h2>
      <p></p>
      <p></p>
      <p>У вас есть {{ skillPoints }}</p>
    </div>
    <div class="content box-shadow text-center">
      <div class="content box-shadow text-center">
                    <h4 class="ntm">Выберите Классовые навыки</h4>
                    <div class="checkbox-toolbar">
                        <span v-for="classSkill in klass.classSkills" :key="'bonus-skills-' + classSkill">
                            <input 
                                type="checkbox" 
                                :id="'classSkill-checkbox-' + classSkill" 
                                :value="classSkill" 
                                v-model="trainedSkills"  
                                :disabled="trainedSkills.length >= 1 && trainedSkills.indexOf( classSkill ) === -1" 
                                @change="$emit( 'set-my-classSkill', trainedSkills ); updateSkill()"
                                />
                            <label :for="'classSkill-checkbox-' + classSkill">{{classSkill}}</label>
                        </span>
                    </div>
                </div>
      
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
                <div class="pretty p-icon p-curve p-pulse" style="font-size: 2rem">
                  <input
                    type="checkbox"
                    :id="skill.name" 
                    v-model="skill.trained"
                    :value="skill"

                    @change='updateCalc(skill)'
                    :disabled="skillNumber >= skillPoints && skill.trained == false"
                    
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
      myskills: null,
      skillList: skillData,
      trainedSkills: [],
      skillNumber: 0,
      Skill: "",
      Profiency: {
                Trained: 2,
                Expert: 4,
                Master: 6,
                Legenda: 8,
      },
    };
  },
  props: {
    race: Object,
    klass: Object,
    abilities: Object,
    name: String,
    pronouns: Object,
    skills: Object,
  },
  computed: {
    skillPoints: function() {
            return parseInt(this.klass.skillPoints) + parseInt(this.calcBonus(this.abilities.интеллект));;
            
        }
  },
  methods: {
    updateSkill: function () {
      let name = this.skillList[this.trainedSkills[0]] ? this.skillList[this.trainedSkills[0]].name : 0;   
      if(this.trainedSkills[0] == name)
      {
        document.getElementById(name).disabled = true;
        this.Skill = name;
        this.skillList[this.trainedSkills[0]].trained = true;
        // return true;
      }
      else
      {
        document.getElementById(this.Skill).disabled = false;
        this.skillList[this.Skill].trained = false;
        
        // return false;
      }
    },
    calcBonus: function (score) {
      var bonus = Math.floor((score - 10) / 2);
      if (bonus > 0) {
        return "+" + bonus;
      } else {
        return bonus;
      }
    },
    calcProf: function ( prof )
        {
            return this.Profiency[prof];

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
      this.skills[skill.name] = bonus;
      // NumberSkill--;


      return bonus;
    },
    isKlassSkill: function (skill) {
      if (skill.trained) {
        this.skillNumber--;
        let abilityBonus = this.calcBonus(this.abilities[skill.ability]);

        let bonus = Number(abilityBonus);
        this.skills[skill.name] = bonus + 3;
        return false;
    }
      else{
        let abilityBonus = this.calcBonus(this.abilities[skill.ability]);
        let bonus = Number(abilityBonus);
        this.skills[skill.name] = bonus;
      }
    },
    updateCalc: function (skill) {
      let abilityBonus = this.calcBonus(this.abilities[skill.ability]);

      let bonus = Number(abilityBonus);
 
    if (skill.trained) {
      
      this.skillNumber++;
    } else {
      this.skillNumber--;
    }


      if(skill.trained == true)
      {
        bonus = bonus + 3;
        // NumberSkill++;
      }
      this.skills[skill.name] = bonus;
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