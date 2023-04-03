
<template>
    <div id="select-feats" class="main-col-content">
        <div class="content">
            <h2 class="section-header">Возьмите Черту!</h2>
            <!-- <p>At level one, all adventurers gain a feat<span v-if="race.race === 'human'">. Так как {{name}} человек, {{pronouns.they}} может выбрать одну Черту</span>!</p> -->
        </div>
        <div v-if="featList[klass.name]" class="content text-center box-shadow">
            <h4>Выберите {{maxFeats}} {{maxFeats > 1 ? 'черты' : 'черту'}} из списка {{maxFeats > 1 ? 'категорий' : 'категории'}}.</h4>
            <h3>Боевая черта</h3>
            <div class="checkbox-toolbar">
                <span v-for="feat in featList[klass.name]" :key="'feat-' + feat.name">
                    <input 
                        type="checkbox" 
                        :id="'feat-checkbox-' + feat.name" 
                        :value="feat" 
                        v-model="checkedClassFeats" 
                        :disabled="checkedClassFeats.length >= 1 && checkedClassFeats.indexOf( feat ) === -1" 
                        @change="$emit( 'set-my-feats',  checkedClassFeats ), updateFeats(feat)"
                    >
                    <label :for="'feat-checkbox-' + feat.name" class="feat-checkbox"><b>{{feat.name}}</b><br>{{feat.desc}}</label>
                </span>
            </div>
            <!-- <h3>Черты навыка</h3>
            <div class="checkbox-toolbar">
                <span v-for="feat in featList.skill" :key="'feat-' + feat.name">
                    <input 
                        type="checkbox" 
                        :id="'feat-checkbox-' + feat.name" 
                        :value="feat" 
                        v-model="checkedFeats" 
                        :disabled="checkedSkillFeats.length >= 1 && checkedFeats.indexOf( feat ) === -1" 
                        @change="$emit( 'set-my-feats', checkedFeats )"
                    >
                    <label :for="'feat-checkbox-' + feat.name" class="feat-checkbox"><b>{{feat.name}}</b><br>{{feat.desc}}</label>
                </span>
            </div> -->

        </div>
    </div>
</template>

<script>
import featData from '../data/feats.json';

export default {

    data: function () {
        return {
            featList: featData,
            checkedClassFeats: [],
            checkedSkillFeats: []
        };
    },
    props: {
        race: Object,
        klass: Object,
        abilities: Object,
        name: String,
        pronouns: Object,
        feats: Array,
    },
    computed: {
        maxFeats: function() {
            return (this.race.race === 'human') ? 2 : 1;
        }
    },
    methods: {
        updateFeats: function(feat)
        {
              if(this.checkedClassFeats.length > 0) 
                this.feats.push(feat);
              else
              this.feats.pop(feat);
        }
    }
       
}
</script>

<style lang="scss">
th {
    text-align: center;
}

.checkbox-toolbar{
    align-items: flex-start;
}

.feat-checkbox{
    width: 31%;
    min-height: 118px;
    font-weight: 400;


    b{
        font-weight: 700;
        font-size: 18px;
    }
}
</style>