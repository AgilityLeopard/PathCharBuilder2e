<template>
    <div id="select-background" class="main-col-content ">
        <div class="content">
            <div class="content box-shadow ">
                <h4 class="ntm text-center">Выберите Предысторию</h4>

                <div id="radio-toolbar select-background content" class="accord-body content-back">
                    <div class="checkbox-toolbar label-back">
                        <span v-for="(backgrounds, backName) in backgroundList">
                            <input type="radio" :id="'select-background-' + backName" :value="backgrounds"
                                v-model="checkedBacks" @change="updateRace" />
                            <label :for="'select-background-' + backName" :key="backName">{{ backgrounds.name }}</label>

                        </span>
                    </div>

                    <div v-if="checkedBacks" class="content content-70">
                        <strong>Описание:</strong>
                        <p> {{ checkedBacks.description }}</p>
                        
                        <p><strong>Обучен:</strong> {{ checkedBacks.lore }} и {{ checkedBacks.skills }}</p>
                        <div v-if="checkedBacks.freeAbility2" class="content box-shadow text-center"
                            style="margin-bottom: 1rem;">
                            <h4 class="ntm">Выберите характеристику.</h4>
                            <div class="radio-toolbar select-background__select-ability content">
                                <template v-for="(score, ability) in checkedBacks.freeAbility2">
                                    <input v-model="freeAbility2" :key="'select-free2-' + ability + '-input'" type="radio"
                                        name="freeAbility2" :id="'select-free2-' + ability" :value="ability"
                                        @change="updateFreeAbility2">
                                    <label :for="'select-free2-' + ability" :key="'select-free2-' + ability + '-label'">
                                        <div>
                                            {{ ability }}
                                        </div>
                                    </label>
                                </template>
                            </div>

                        </div>

                        <div v-if="checkedBacks.freeAbility1" class="content box-shadow text-center" style="margin-bottom: 1rem;">
                            <h4 class="ntm">Выберите характеристику.</h4>
                            <div class="radio-toolbar select-background__select-ability content">
                                <template v-for="(score, ability) in checkedBacks.abilities">
                                    <input v-model="freeAbility1" :key="'select-free1-' + ability + '-input'" type="radio"
                                        name="freeAbility1" :id="'select-free1-' + ability" :value="ability"
                                        @change="updateFreeAbility1">
                                    <label :for="'select-free1-' + ability" :key="'select-free1-' + ability + '-label'">
                                        <div>
                                            {{ ability }}
                                        </div>
                                    </label>
                                </template>
                            </div>
                        </div>
                    </div>



                    </div>

                </div>

            </div>
        </div>
</template>

<script>

import backgroundData from '../data/backgrounds.json';

export default {
    //:disabled="checkedBacks.length >= 1 && checkedBacks.indexOf( backgrounds.name ) === -1" 
    data: function () {
        return {
            myRace: null,
            backgroundList: backgroundData,
            BoostAbility1: "",
            BoostAbility2: "",
            freeAbility2: "",
            freeAbility1: "",
            checkedBacks: null,
            myBack: null,
        };
    },
    props: {
        propRace: Object,
        abilities: Object,
        name: String,
        pronouns: Object,
        propbackground: Object,
    },
    computed: {
        intBonus: function () {
            return Math.floor((this.abilities.интеллект - 10) / 2);
        }
    },
    created() {
        if (null != this.propbackground) {
            this.checkedBacks = this.propbackground;
        }
    },
    methods: {
        isPositive: function (value) {
            return value > 0;
        },
        isNegative: function (value) {
            return value < 0;
        },
        updateRace: function () {
            this.checkedLanguages = [];
            this.freeAbility1 = null;
            this.freeAbility2 = null;
            this.$emit('set-background', this.checkedBacks);
        },
        updateFreeAbility1: function () {
            if (this.checkedBacks.freeAbility1) {
                for (let ability in this.checkedBacks.abilities) {
                    if (this.checkedBacks.abilities.hasOwnProperty(ability) && ability != this.freeAbility2) {
                        this.checkedBacks.abilities[ability] = 0;
                    }
                }
                this.checkedBacks.abilities[this.freeAbility1] = 2;
            }
        },
        updateFreeAbility2: function () {
            if (this.checkedBacks.freeAbility2) {
                for (let ability in this.checkedBacks.abilities) {
                    if (this.checkedBacks.abilities.hasOwnProperty(ability) && ability != this.freeAbility1) {
                        this.checkedBacks.abilities[ability] = 0;
                    }
                }
                this.checkedBacks.abilities[this.freeAbility2] = 2;
            }
        }
    }
}
</script>

<style lang="scss">

.label-back {
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    /* прокрутка по вертикали */
    width: 400px;
    /* ширина нашего блока */
    height: 400px;
    /* высота нашего блока */
}

.content-back {
    display: flex;
    flex-direction: row;
    // flex: 31%;
}

.content-70 {
    // display: flex;
    // flex-direction: row;
    flex: 91%;
}

.select-background {
    display: flex;
    align-items: stretch;
    flex-wrap: wrap;


    label {
        font-weight: normal;
        flex: 1 1 71%;
        display: flex;




        @media only screen and (max-width: 767px) {
            flex: 1 1 48%;
        }

        &>div {
            top: 100%;
            transform: translateY(-100%);
            position: relative;
            text-align: center;
            font-weight: bold;
            text-transform: capitalize;
        }
    }

    &__select-background {
        display: flex;
        align-items: stretch;
        flex-wrap: wrap;


        label {
            font-weight: normal;
            flex: 1 1 31%;
            display: relative;


            &>div {
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

.ability-bonuses>span {
    text-transform: capitalize;
}
</style>