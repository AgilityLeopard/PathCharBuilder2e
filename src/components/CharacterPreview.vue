<template>
	<div id="character-preview" class="content box-shadow">
        <h3 class="ntm">{{name}}</h3>
        <strong v-if="race" class="cap">{{race.race}}</strong>
        <strong v-if="klass" class="cap">{{klass.name}}</strong>

        <h5>Оборона</h5>
        <div><strong>Хиты</strong> {{hp}}</div>
        <!-- <div style="display: flex; justify-content: space-between;">
            <div><strong>AC:</strong> {{ac}}</div>
            <div><strong>TAC:</strong> {{touch}}</div>
            <div><strong>FFAC:</strong> {{ff}}</div>
        </div> -->
        <div style="display: flex; justify-content: space-between;">
            <div><strong>Рефлекс:</strong> {{ref}}</div>
            <div><strong>Стойкость:</strong> {{fort}}</div>
            <div><strong>Воля:</strong> {{will}}</div>
        </div>



        <h5>Оборона</h5>
        <b>Скорость</b> {{ speed }}<br>
        <!-- <b>Ближний бой</b> {{ mab }}<br>
        <b>Дальний бой</b> {{ rab }}<br> -->

        <template v-if="myLanguages">
            <h5>языки</h5>
            <span 
                v-for="language in myLanguages" 
                :key="'preview-lang-traits-' + language" 
                class="preview__lang-trait csli"
                data-toggle="tooltip" 
                data-placement="top" 
                :title="language"
            >{{language}}</span>
        </template>

        <template v-if="mySkills">
            <h5>Навыки</h5>
            <div class="preview__skills hidden-md-down">
            <div v-for="(score, skills) in skillsFormatted" :key="'preview-desktop-' + skills" class="preview__skills">
                <strong>{{ skills }}:</strong> {{ score }} 
            </div>
        </div>
        <div class="preview__skills hidden-lg-up">
            <div v-for="(score, skills) in mySkills" :key="'preview-mobile-' + skills" class="preview__skills">
                <strong>{{ skills }}:</strong> {{ score }} 
            </div>
        </div>
        </template>

        <h5>Способности</h5>
        <div class="preview__abilities hidden-md-down">
            <div v-for="(score, ability) in abilitiesFormatted" :key="'preview-desktop-' + ability" class="preview__ability">
                <strong>{{ ability.substring( 0, 3 ) }}:</strong> {{ score }} ({{ calcBonus( score ) }})
            </div>
        </div>
        <div class="preview__abilities hidden-lg-up">
            <div v-for="(score, ability) in abilities" :key="'preview-mobile-' + ability" class="preview__ability">
                <strong>{{ ability.substring( 0, 3 ) }}:</strong> {{ score }} ({{ calcBonus( score ) }})
            </div>
        </div>

        <template v-if="race">
            <h5>Расовые черты</h5>
            <span 
                v-for="(trait) in race.racialTraits" 
                :key="'preview-racial-traits-' + trait.name" 
                class="preview__racial-trait csli"
                data-toggle="tooltip" 
                data-placement="top" 
                :title="trait.desc"
            >{{trait.name}}</span>
        </template>

        
        <template v-if="klass">
            <h5>Классовые черты</h5>
            <span 
                v-for="(trait) in klass.abilities" 
                :key="'preview-klass-traits-' + trait.name" 
                class="preview__klass-trait csli"
                data-toggle="tooltip" 
                data-placement="top" 
                :title="trait.description"
            >{{trait.name}}</span>
        </template>

        <template v-if="feats">
            <h5>Черты</h5>
            <span 
                v-for="feat in feats" 
                :key="'preview-feats-' + feat.name" 
                class="preview__feat csli"
                data-toggle="tooltip" 
                data-placement="top" 
                :title="feat.desc"
            >{{feat.name}}</span>
        </template>

    </div>
</template>

<script>
export default {
	data: function () {
        return{
            dummy: '',
            Profiency: {
                Trained: 2,
                Expert: 4,
                Master: 6,
                Legenda: 8,
      },
        };
    },
    props: {
        name: String,
        abilities: Object,
        race: Object,
        klass: Object,
        favouredKlass: String,
        feats: Array,
        myLanguages: Array,
        mySkills: Object,
        level: Number,
    },
    methods: {
        calcBonus: function( score ) {
            var bonus = Math.floor( ( score - 10 ) / 2 );
            if ( bonus > 0 ) {
                return '+' + bonus;
            } else {
                return bonus;
            }
        }
    },
    computed: {
        abilitiesFormatted: function () {
            return {
                сил: this.abilities.сила,
                инт: this.abilities.интеллект,
                лов: this.abilities.ловкость,
                муд: this.abilities.мудрость,
                тел: this.abilities.телосложение,
                хар: this.abilities.харизма,
            }
        },
        skillsFormatted: function () {
            return {
                Акробатика: this.mySkills.Акробатика,
                Аркана: this.mySkills.Аркана,
                Атлетика: this.mySkills.Атлетика,
                Ремесло: this.mySkills.Ремесло,
                Обман: this.mySkills.Обман,
                Дипломатия: this.mySkills.Дипломатия,
                Запугивание: this.mySkills.Запугивание,
                Медицина: this.mySkills.Медицина,
                Природа: this.mySkills.Природа,
                Оккультизм: this.mySkills.Оккультизм,
                Выступление: this.mySkills.Выступление,
                Религия: this.mySkills.Религия,
                Общество: this.mySkills.Общество,
                Скрытность: this.mySkills.Скрытность,
                Выживание: this.mySkills.Выживание,
                Воровство: this.mySkills.Воровство,
            }
        },
        hp: function () {
            let klassBonus = this.klass ? parseInt(this.klass.hp) : 6;
            let hp = klassBonus + parseInt(this.calcBonus(this.abilities.телосложение));
            return ( 'hp' === this.favouredKlass ) ? hp + 1 : hp;
        },
        ac: function () {
            let ac = 10 + parseInt(this.calcBonus(this.abilities.ловкость));
            return ac;
        },
        // touch: function () {
        //     let ac = 10 + parseInt(this.calcBonus(this.abilities.ловкость));
        //     return ac;
        // },
        // ff: function () {
        //     let ac = 10;
        //     return ac;
        // }, 
        ref: function() {
            // let racialBonus = (this.race && this.race.bonuses.saves) ? this.race.bonuses.saves : 0;
            let klassBonus = this.klass ? parseInt(this.Profiency[this.klass.ref]) : 0;

            let ref = klassBonus + parseInt(this.calcBonus(this.abilities.ловкость)) + parseInt(this.level);

            if ( ref > 0) {
                return '+' + ref;
            }
            else {
                return ref;
            }
        },
        fort: function() {
            // let racialBonus = (this.race && this.race.bonuses.saves) ? this.race.bonuses.saves : 0;
            let klassBonus = this.klass ? parseInt(this.Profiency[this.klass.fort]) : 0;
            let fort = klassBonus + parseInt(this.calcBonus(this.abilities.телосложение)) + + parseInt(this.level);;
            if ( fort > 0) {
                return '+' + fort;
            }
            else {
                return fort;
            }
        },
        will: function() {
            // let racialBonus = (this.race && this.race.bonuses.saves) ? this.race.bonuses.saves : 0;
            let klassBonus = this.klass ? parseInt(this.Profiency[this.klass.will]) : 0;
            let will = klassBonus + parseInt(this.calcBonus(this.abilities.мудрость)) + + parseInt(this.level);;
            if ( will > 0) {
                return '+' + will;
            }
            else {
                return will;
            }
        },
        mab: function() {
            let klassBonus = this.klass ? parseInt(this.klass.bab) : 0;
            let mab = klassBonus + parseInt(this.calcBonus(this.abilities.сила));
            if ( mab >= 0) {
                return '+' + mab;
            }
            else {
                return mab;
            }
        },
        rab: function() {
            let klassBonus = this.klass ? parseInt(this.klass.bab) : 0;
            let rab = klassBonus + parseInt(this.calcBonus(this.abilities.ловкость));
            if ( rab >= 0) {
                return '+' + rab;
            }
            else {
                return rab;
            }
        },
        speed: function() {
            return (this.race) ? this.race.speed : 30;
        },
        language: function() {
            return (this.myLanguages);
        },
        skill: function() {
            return (this.skills);
        },
    }
};
</script>

<style lang="scss">
    #character-preview {
        margin-bottom: 2rem;
    }

    .preview {
        &__abilities {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            
            @media only screen and (max-width: 991px) {
                justify-content: flex-start;
            }

            .preview__ability {
                padding: .25rem;

                @media only screen and (min-width: 992px) {
                    width: 47%;
                }

                strong {
                    text-transform: capitalize;
                }
            }
        }

        &__racial-trait{
            &:hover{
                background: var(--secondary-color-light);
                font-weight: bold;
            }
        }
    }

    @media only screen and (max-width: 991px){
    .hidden-md-down {
      display: none;
    }
  }
  @media only screen and (min-width: 992px){
    .hidden-lg-up {
      display: none;
    }
  }
</style>