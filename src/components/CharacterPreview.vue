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
        <b>Ближний бой</b> {{ mab }}<br>
        <b>Дальний бой</b> {{ rab }}<br>

        <template v-if="myLanguages">
            <h5>языки</h5>
            <span 
                v-for="language in myLanguages" 
                :key="'preview-lang-traits-' + lang" 
                class="preview__lang-trait csli"
                data-toggle="tooltip" 
                data-placement="top" 
                :title="lang"
            >{{language}}</span>
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
            >{{trait.name}}</span>
        </template>

    </div>
</template>

<script>
export default {
	data: function () {
        return{
            dummy: ''
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
        hp: function () {
            let klassBonus = this.klass ? parseInt(this.klass.hp) : 6;
            let hp = klassBonus + parseInt(this.calcBonus(this.abilities.телосложение));
            return ( 'hp' === this.favouredKlass ) ? hp + 1 : hp;
        },
        ac: function () {
            let ac = 10 + parseInt(this.calcBonus(this.abilities.ловкость));
            return ac;
        },
        touch: function () {
            let ac = 10 + parseInt(this.calcBonus(this.abilities.ловкость));
            return ac;
        },
        ff: function () {
            let ac = 10;
            return ac;
        }, 
        ref: function() {
            let racialBonus = (this.race && this.race.bonuses.saves) ? this.race.bonuses.saves : 0;
            let klassBonus = this.klass ? parseInt(this.klass.ref) : 0;
            let ref = klassBonus + parseInt(this.calcBonus(this.abilities.ловкость)) + racialBonus;
            if ( ref > 0) {
                return '+' + ref;
            }
            else {
                return ref;
            }
        },
        fort: function() {
            let racialBonus = (this.race && this.race.bonuses.saves) ? this.race.bonuses.saves : 0;
            let klassBonus = this.klass ? parseInt(this.klass.fort) : 0;
            let fort = klassBonus + parseInt(this.calcBonus(this.abilities.телосложение)) + racialBonus;
            if ( fort > 0) {
                return '+' + fort;
            }
            else {
                return fort;
            }
        },
        will: function() {
            let racialBonus = (this.race && this.race.bonuses.saves) ? this.race.bonuses.saves : 0;
            let klassBonus = this.klass ? parseInt(this.klass.will) : 0;
            let will = klassBonus + parseInt(this.calcBonus(this.abilities.мудрость)) + racialBonus;
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