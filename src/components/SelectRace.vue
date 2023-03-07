
<template>
    <div id="select-abilities" class="main-col-content">
        <div class="content">
            <h2 class="section-header">Теперь давайте выберем расу!</h2>
            <p>В Pathfinder раса является фундаментальной частью каждого персонажа. Раса смешивает биологию и культуру, а затем переводит эти концепции в расовые черты. Черты расы, история и культура формируют вашего персонажа. Это верно независимо от того, играете ли вы за стереотипы расы или против них.
            <p>Выбор расы вашего персонажа — одно из наиболее важных решений, которое вам нужно будет принять. Каждая раса лучше всего подходит для определенного типа роли — дварфы лучше борются, чем колдуны, а полурослики не так хороши, как полуорки, как варвары. Представленные здесь расы обладают совершенно разными способностями, характерами и обществами.
но в то же время ни одна из рас здесь не отклоняется слишком далеко от человечества, и все их способности примерно равны и сбалансированы.</p>
        </div>
        <div class="content box-shadow text-center">
            <h4 class="ntm">Выберите расу.</h4>
            <div class="radio-toolbar select-race content">
                <template v-for="(race, raceName) in races">
                    <input 
                        v-model="myRace" 
                        :key="raceName + '-input'" 
                        type="radio" 
                        name="race" 
                        :id="'select-race-' + raceName" 
                        :value="race" 
                        @change="updateRace">
                    <label :for="'select-race-' + raceName"  :key="raceName">
                        <div>
                            <img class="select-race__image" :src="'src/assets/' + race.image"><br>
                            {{race.race}}
                        </div>
                    </label>
                </template>
            </div>
        </div>
        <div v-if="myRace" class="content">
            <h2 class="race-title cap">{{myRace.race}}</h2>
            <p>{{myRace.description}}</p>
            <h3>Черты</h3>
            <p v-if="!myRace.freeAbility1" class="ability-bonuses">
                <strong>Характеристики:</strong>
                <span v-for="(mod, ability) in myRace.abilities" :key="'race-ability-bonus-' + ability">
                    <template v-if="mod > 0"> +{{mod}} {{ability}}, </template>
                </span>
                but
                <span v-for="(mod, ability) in myRace.abilities" :key="'race-ability-penalty-' + ability">
                    <template v-if="mod < 0">{{mod}} {{ability}}</template>
                </span>
            </p>
            <div v-else>
                <p><strong>Очки характеристик:</strong> <span class="cap">{{myRace.plural}}</span> получают буст +2 на ваш выбор.</p>
                <div v-if="myRace.freeAbility1" class="content box-shadow text-center" style="margin-bottom: 1rem;">
                    <h4 class="ntm">Выберите характеристику.</h4>
                    <div class="radio-toolbar select-race__select-ability content">
                    <template v-for="(score, ability) in myRace.abilities">
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
                <p><strong>Очки характеристик:</strong> <span class="cap">{{myRace.plural}}</span> получают буст +2 на ваш выбор.</p>
                <div v-if="myRace.freeAbility2" class="content box-shadow text-center" style="margin-bottom: 1rem;">
                    <h4 class="ntm">Выберите характеристику.</h4>
                    <div class="radio-toolbar select-race__select-ability content">
                    <template v-for="(score, ability) in myRace.abilities">
                        <input 
                            v-model="freeAbility2" 
                            :key="'select-free2-' + ability + '-input'" 
                            type="radio" 
                            name="freeAbility2" 
                            :id="'select-free2-' + ability" 
                            :value="ability" 
                            @change="updateFreeAbility2">
                        <label :for="'select-free2-' + ability"  :key="'select-free2-' + ability + '-label'">
                            <div>
                                {{ability}}
                            </div>
                        </label>
                    </template>
                </div>
                
                </div>
            </div>
            <p><strong>Размер: </strong> {{myRace.size}} 
            
            <span v-if="myRace.size == 'маленький'" class="caption"> <br></span></p>
            <p><strong>Скорость: </strong> {{myRace.speed}} футов</p>
            <p>
                <strong>Языки: </strong>
                <span v-for="language in myRace.languages" :key="'racial-language-' + language" class="csli">{{language}}</span>
            </p>
            <div v-if="intBonus > 0">
                <p>
                    <strong>Бонусные языки: </strong> 
                    Вы {{name}} настолько интеллектуальны, что можете позволить {{intBonus}} {{ intBonus == 1 ? 'бонусного языка' : 'бонусных языков' }} для {{pronouns.them}} изучения.
                </p>
                <div class="content box-shadow text-center">
                    <h4 class="ntm">Выберите {{intBonus}} {{ intBonus == 1 ? 'бонусный язык' : 'бонусных языка' }}.</h4>
                    <div class="checkbox-toolbar">
                        <span v-for="language in myRace.bonusLanguages" :key="'bonus-language-' + language">
                            <input 
                                type="checkbox" 
                                :id="'language-checkbox-' + language" 
                                :value="language" 
                                v-model="checkedLanguages" 
                                :disabled="checkedLanguages.length >= intBonus && checkedLanguages.indexOf( language ) === -1" 
                                @change="$emit( 'set-my-languages', myRace.languages.concat( checkedLanguages ) )"
                            >
                            <label :for="'language-checkbox-' + language">{{language}}</label>
                        </span>
                    </div>
                </div>
            </div>
            <h3>Расовые черты</h3>
            <template v-if="myRace.racialTraits && myRace.racialTraits.length > 0">
                <p v-for="trait in myRace.racialTraits" :key="'racial-trait-' + myRace.race + trait.name">
                    <strong>{{trait.name}}:</strong> {{trait.desc}}
                </p>
            </template>
        </div>
    </div>
</template>

<script>
import raceData from '../data/races.json';

export default {

    data: function () {
        return {
            myRace: null,
            races: raceData,
            BoostAbility1: "",
            BoostAbility2: "",
            freeAbility2: "",
            freeAbility1: "",
            checkedLanguages: []
        };
    },
    props: {
        propRace: Object,
        abilities: Object,
        name: String,
        pronouns: Object,
    },
    computed: {
        intBonus: function() {
            return Math.floor( ( this.abilities.интеллект - 10 ) / 2 );
        }
    },
     created() {
        if ( null != this.propRace) {
            this.myRace = this.propRace;
        }
    },
    methods: {
        isPositive: function ( value ) {
            return value > 0;
        },
        isNegative: function ( value ) {
            return value < 0;
        },
        updateRace: function() {
            this.checkedLanguages = [];
            this.freeAbility1 = null;
            this.freeAbility2 = null;
            this.$emit('set-race', this.myRace);
        },
        updateFreeAbility1: function() {
             if ( this.myRace.freeAbility1 ) {
                 for (let ability in this.myRace.abilities){
                     if(this.myRace.abilities.hasOwnProperty(ability) && ability != this.freeAbility2){
                         this.myRace.abilities[ability] = 0;
                     }
                 }
                this.myRace.abilities[ this.freeAbility1 ] = 2;
             
             }
        },
        updateFreeAbility2: function() {
            if ( this.myRace.freeAbility2 ) {
                 for (let ability in this.myRace.abilities){
                     if(this.myRace.abilities.hasOwnProperty(ability) && ability != this.freeAbility1){
                         this.myRace.abilities[ability] = 0;
                     }
                 }
                this.myRace.abilities[ this.freeAbility2 ] = 2;
             }
        }
    }
}
</script>

<style lang="scss">

.select-race {
    display: flex;
    align-items: stretch;
    flex-wrap: wrap;

    label {
        font-weight: normal;
        flex: 1 1 31%;
        display: relative;

        @media only screen and (max-width: 767px) {
            flex: 1 1 48%;
        }

        & > div {
            top: 100%;
            transform: translateY(-100%);
            position: relative;
            text-align: center;
            font-weight: bold;
            text-transform: capitalize;
        }
    }

    &__select-ability {
        display: flex;
        align-items: stretch;
        flex-wrap: wrap;

        label {
        font-weight: normal;
        flex: 1 1 31%;
        display: relative;
        
        & > div {
            top: 100%;
            transform: translateY(-100%);
            position: relative;
            text-align: center;
            font-weight: bold;
            text-transform: capitalize;
        }
        }
    }

    &__image {
        margin-bottom: 1rem;
        width: 92px;
        height: auto;        
    }
}

.ability-bonuses > span {
    text-transform: capitalize;
}
</style>