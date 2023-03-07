
<template>
    <div id="select-equipment" class="main-col-content">
        <div class="content">
            <h2 class="section-header">В разработке!</h2>
            <h3>Золото: {{gold}}</h3>
            <h3>Оружие</h3>
            <h3>Доспехи</h3>
                <div class="radio-toolbar select-armor content">
                    <template v-for="armor in armors" >
                        <input 
                            v-model="deity" 
                            
                            type="radio" 
                            :name="armor.name + '-input'" 
                            :id="'select-deity-' + deityName" 
                            :value="myArmor" 
                            
                        >
                        <label  for="armor.name + '-input'">
                            <b>{{armor.name}}</b><br>                    
                            <b>Бонус Доспеха</b> {{armor.bonus}}<br>
                            <b>Максимальный бонус Ловкости</b> {{armor.dex}}<br>
                            <b>Тип</b> {{armor.type}}<br>
                            <b>Стоимость</b> {{armor.cost}} зм<br>
                            <b>Пенальти Доспеха</b> {{armor.acp}}
                            <b>Вес</b> {{armor.weight}}
                        </label>
                    </template>
                </div>
            <h3>Щит</h3>
            <h3>Снаряжение</h3>
        </div>
    </div>
</template>

<script>
import armorData from '../data/armor.json';

export default {

    data: function() {
        return {
            armors: armorData,
            myArmor: {}
        };
    },
    props: {
        race: Object,
        klass: Object,
        abilities: Object,
        name: String,
        pronouns: Object,
    },
    computed: {
        gold: function() {
            let gold = this.klass.gold;
            let armor = this.myArmor ? this.myArmor.cost : 0;
            return gold;
        }
    },
    methods: {
        updatArmor: function() {
        }
    } 
}
</script>

<style lang="scss">
</style>