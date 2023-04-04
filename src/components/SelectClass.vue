
<template>
    <div id="select-classAbilities" class="main-col-content">
        <div class="content">
            <h2 class="section-header">Какой класс вы выбирите?</h2>
            <!-- <p>{{name}}'s class is one of {{pronouns.their}} most defining features. Здесь должен быть текст {{pronouns.their}} classAbilities and gives {{pronouns.them}} a specific role in any adventuring party.</p> -->
        </div>
        <div class="content box-shadow text-center">
            <h4 class="ntm">Выберите класс.</h4>
            <div class="radio-toolbar select-race content">
                <template v-for="(klass, klassName) in klasses">
                    <input 
                        v-model="myKlass" 
                        :key="klassName + '-input'" 
                        type="radio" 
                        name="class" 
                        :id="'select-class-' + klassName" 
                        :value="klass" 
                        @change="updateKlass">
                    <label :for="'select-class-' + klassName"  :key="klassName">
                        <div>
                            <img class="select-race__image" :src="'src/assets/' + klass.image"><br>
                            {{klass.name}}
                        </div>
                    </label>
                </template>
            </div>
        </div>

        <div v-if="myKlass" class="content content-back overflow">
            <div ><h2 class="klass-title cap">{{myKlass.name}}</h2></div>
            <div ><p>{{myKlass.description}}</p></div>
            <div class="content-back"><h3>Стандартные особенности</h3></div>
            <div class="std-klass-traits nested-items">
                <p class="std-klass-trait">
                    <strong  data-placement="top" data-toggle="tooltip" :title="'Количество ' + name + ' имеющихся сейчас'">Хиты</strong>
                    <span  data-placement="top" data-toggle="tooltip" title="Это значение включает ваш модификатор Телосложения.">
                        {{hp}}
                    </span>
                </p>
                <div v-if="myKlass.freeAbility1" class="content box-shadow text-center " style="margin-bottom: 1rem;">
                    <h4 class="ntm">Выберите характеристику.</h4>
                    <div class="radio-toolbar select-class__select-ability content ">
                    <template v-for="(score, ability) in myKlass.classAbilities">
                        <input 
                            v-model="freeAbility1" 
                            :key="'select-free1-' + ability + '-input'" 
                            type="radio" 
                            name="freeAbility1" 
                            :id="'select-free1-' + ability" 
                            :value="ability" 
                            
                            @change="updateFreeAbility1">
                        <label :for="'select-free1-' + ability"  :key="'select-free1-' + ability + '-label'">
                            <div >
                                {{ability}}
                            </div>
                        </label>
                    </template>
                </div>
                </div>
    
                <!-- <p class="std-klass-trait">
                    <strong data-placement="top" data-toggle="tooltip" :title="'Значение ' + pronouns.their + ' ближней атаки'">Ближний бой:</strong>
                    <span data-placement="top" data-toggle="tooltip" title="Это значение включает ваш модификатор Силы.">
                        {{mab}}
                    </span>
                </p>
                <p class="std-klass-trait">
                    <strong data-placement="top" data-toggle="tooltip" :title="'Значение ' + pronouns.their + ' дальнобойной атаки'">Дальний бой:</strong>
                    <span data-placement="top" data-toggle="tooltip" title="Это значение включает ваш модификатор Ловкости.">
                        {{rab}}
                    </span>
                </p> -->
            </div>
            <div class="std-klass-traits content-back">
            <div class="content-back"> <p class="std-klass-trait" data-placement="top" data-toggle="tooltip" title="">
                    <strong>Рефлекс:</strong>
                    <span data-placement="top" data-toggle="tooltip" title="Это значение включает ваш модификатор Ловкости.">
                        {{ref}}
                    </span>
                </p></div>
                <div class="content-back"><p class="std-klass-trait ">
                    <strong data-placement="top" data-toggle="tooltip" title="">Стойкость:</strong>
                    <span data-placement="top" data-toggle="tooltip" title="Это значение включает ваш модификатор Телосложения.">
                        {{fort}}
                    </span>
                </p></div>
                <div class="content-back"><p class="std-klass-trait">
                    <strong data-placement="top" data-toggle="tooltip" title="">Воля:</strong>
                    <span data-placement="top" data-toggle="tooltip" title="Это значение включает ваш модификатор Мудрости.">
                        {{will}}
                    </span>
                </p></div>
            </div>
           
            <div class="std-klass-traits content-back">
                <div class="content-back"><p class="std-klass-trait">
                    <!-- <strong data-placement="top" data-toggle="tooltip" title="You will be able to train this number of skills.">Skill Points:</strong> -->
                    <!-- <span data-placement="top" data-toggle="tooltip" title="Это значение включает ваш модификатор Интеллекта.">
                        {{skillPoints}}
                    </span> -->
                </p></div>
                <div class="content-back"><p class="std-klass-trait std-klass-trait--class-skills">
                    <strong data-placement="top" data-toggle="tooltip" title="Если вы тренируете любой из этих навыков, вы получите дополнительный бонус +3 к этому навыку.">Классовые навыки:</strong>
                    <span class="no-hover">
                        <span v-for="skill in myKlass.classSkills" :key="skill" class="csli">
                            {{skill}}</span>
                    </span>
                </p></div>
            </div>
            <div class="content-back"><div class="std-klass-traits">
                <p class="std-klass-trait">
                    <strong data-placement="top" data-toggle="tooltip" title="Вы сможете потратить это на оборудование.">Стартовое богатство:</strong>
                    <span class="no-hover">
                        {{myKlass.gold}} золотых монет
                    </span>
                </p></div>
            </div>
            <div class="content-back"> <h3>Оружие и доспехи</h3>
                    <p>{{myKlass.proficiencyDescription}}</p></div>
            
            <fighter v-if="myKlass.name == 'Fighter'"></fighter>
            <rogue v-if="myKlass.name == 'Rogue'"></rogue>
            <paladin 
                v-if="myKlass.name == 'Paladin'" 
                :name="name" 
                :pronouns="pronouns" 
                :classAbilities="classAbilities" >
            </paladin>
            <cleric 
                v-if="myKlass.name == 'Cleric'"
                @set-deity="setDeity"
                @set-domains="setDomains"
                :name="name" 
                :pronouns="pronouns" 
                :classAbilities="classAbilities"
                >
            </cleric>
            <sorcerer v-if="myKlass.name == 'Sorcerer'"></sorcerer>
            <wizard 
                v-if="myKlass.name == 'Wizard'"
                :name="name" 
                :pronouns="pronouns" 
                :classAbilities="classAbilities">
            </wizard>

            <!-- <h3>Favoured Class Bonus</h3> -->
            <!-- <p>
                {{myKlass.name}} is {{name}}'s' favoured class. At first level and each time {{name}} levels up in this class {{pronouns.they}} will gain either a bonus hit point or a bonus skill point.
            </p> -->
            <!-- <div class="content box-shadow text-center">
                <h4 class="ntm">Select a Favoured Class Bonus</h4>
                <div class="radio-toolbar content">
                    <input 
                        v-model="favouredKlass"
                        type="radio" 
                        name="favoured-klass-hp" 
                        id="favoured-klass-hp" 
                        value="hp" 
                        @change="$emit('set-favoured-klass', favouredKlass)">
                    <label for="favoured-klass-hp">Бонусные очки жизни
                    </label>
                    <input 
                        v-model="favouredKlass"
                        type="radio" 
                        name="favoured-klass-sp" 
                        id="favoured-klass-sp" 
                        value="sp" 
                        @change="$emit('set-favoured-klass', favouredKlass)">
                    <label for="favoured-klass-sp">Bonus Skill Point
                    </label>
                </div>
            </div> -->
        </div>
    </div>
</template>

<script>
import klassData from '../data/klasses.json';
import Fighter from './klasses/Fighter.vue';
import Rogue from './klasses/Rogue.vue';
import Paladin from './klasses/Paladin.vue';
import Cleric from './klasses/Cleric.vue';
import Sorcerer from './klasses/Sorcerer.vue';
import Wizard from './klasses/Wizard.vue';

export default {

    data: function () {
        return {
            myKlass: null,
            klasses: klassData,
            favouredKlass: '',
            BoostAbility1: "",
            BoostAbility2: "",
            freeAbility2: "",
            freeAbility1: "",
            Profiency: {
                Trained: 2,
                Expert: 4,
                Master: 6,
                Legenda: 8,
      },
        };
    },
    props: {
        propKlass: Object,
        classAbilities: Object,
        race: Object,
        name: String,
        pronouns: Object,
        level: Number,
    },
    components: {
        Fighter,
        Rogue,
        Paladin,
        Cleric,
        Sorcerer,
        Wizard,
    },
     created() {
        if ( null != this.propKlass) {
            this.myKlass = this.propKlass;
        }
    },
    computed: {
        hp: function () {
            let hp = parseInt(this.myKlass.hp) + parseInt(this.calcBonus(this.race.abilities.телосложение));
            return ( 'hp' === this.favouredKlass ) ? hp + 1 : hp;
        },
        ref: function() {
            // let racialBonus = this.race.bonuses.saves ? this.race.bonuses.saves : 0;
            let klassBonus = this.myKlass ? parseInt(this.Profiency[this.myKlass.ref]) : 0;
            let ref = klassBonus + parseInt(this.calcBonus(this.race.abilities.ловкость)) + parseInt(this.level);
            // let ref = parseInt(this.calcProf(this.myKlass.ref));
            console.log(parseInt(this.level));

            if ( ref > 0) {
                return '+' + ref;
            }
            else {
                return ref;
            }
        },
        fort: function() {
            let klassBonus = this.myKlass ? parseInt(this.Profiency[this.myKlass.fort]) : 0;
            let fort = klassBonus + parseInt(this.calcBonus(this.race.abilities.ловкость)) + parseInt(this.level);
            if ( fort > 0) {
                return '+' + fort;
            }
            else {
                return fort;
            }
        },
        will: function() {
            let klassBonus = this.myKlass ? parseInt(this.Profiency[this.myKlass.will]) : 0;
            let will = klassBonus + parseInt(this.calcBonus(this.race.abilities.ловкость)) + parseInt(this.level);
            if ( will > 0) {
                return '+' + will;
            }
            else {
                return will;
            }
        },
    },
    methods: {
        updateKlass: function() {
            this.$emit('set-klass', this.myKlass);
        },
        calcBonus: function( score ) {
            var bonus = Math.floor( ( score - 10 ) / 2 );
            if ( bonus > 0 ) {
                return '+' + bonus;
            } else {
                return bonus;
            }
        },
        updateFreeAbility1: function() {

//Нужно перера
 if ( this.myKlass.freeAbility1 ) {
     for (let ability in this.myKlass.classAbilities){ 
         if(this.myKlass.classAbilities.hasOwnProperty(ability)){
            if(this.myKlass.classBuild[ability] == 0)
                this.myKlass.classAbilities[ability] = 0;
            if(this.myKlass.classBuild[ability] == -2)
                this.myKlass.classAbilities[ability] = -2;
         }
     }

     if (this.myKlass.classBuild[this.freeAbility1] == 0)
     {
        this.myKlass.classAbilities[ this.freeAbility1 ] = 2;
     }
     else if (this.myKlass.classBuild[this.freeAbility1] == -2)
     {
        this.myKlass.classAbilities[ this.freeAbility1 ] = 0;
     }
    
 
 }
},
        calcProf: function ( prof )
        {
            return this.Profiency[prof];

        },
        setDeity: function( value ) {
            this.myKlass.deity = value;
            this.updateKlass;
        },
        setDomains: function( value ) {
            this.myKlass.domains = value;
            this.updateKlass;
        },
    }
}
</script>

<style lang="scss">

.std-klass-traits {
    display: flex;
    justify-content: space-between;
    text-align: left;

    .std-klass-trait {
        flex: 1 1 31%;

        > *:hover {
            background: var(--secondary-color-light);
        }
    }

    .std-klass-trait--class-skills {
        flex: 1 1 64%;
        display: flex;

        > strong {
            min-width: 80px;
        }
    }

    .no-hover:hover {
        background: white;
    }
}

.content-back {
    //  display: flex;
    //  flex-direction: column;
    // overflow-y: scroll;
    // flex-wrap: wrap;

    display: flex;
    // width: 56px;
   width: 66vw;
  height: auto;
  box-sizing: border-box;
  margin: 0 auto;
  flex-wrap: wrap;
}

.overflow {
    overflow-y: scroll;
    height: 400px;
}

.nested-items {
  display: flex;
  width: 42%;
  flex-wrap: wrap;
  align-content: space-between;
}


</style>
