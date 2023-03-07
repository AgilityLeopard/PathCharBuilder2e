
<template>
    <div id="select-equipment" class="main-col-content">
        <div class="content">
            <h2 class="section-header">Почти конец!</h2>
            <h3>Мировоззрение</h3>
            <p></p>
            <p><b>Добрый против Злого: </b></p>
            <p><b>Законопослушный против Хаотичного:</b> </p>
        </div>
        <div class="content box-shadow text-center">
            <h4 class="ntm">Выберите Мировоззрение.</h4>
            <p><b>ЗВ разработке:</b> </p>
        </div>
        <div class="content">
            <h3>Рост и ширина</h3>
            <h3>Волосы, Цвет глаз и цвет кожи</h3>
            <label for="hair">Волосы</label>
            <select name="hair">
                <option value="black" style="background: black; color: white;">Черный</option>
                <option value="grey"  style="background: #777; color: white;">Серый</option>
                <option value="silver"  style="background: #ddd;">Сербрянный</option>
                <option value="white">Белый</option>
                <option value="blonde" style="background: #fc6;">Блондинистый</option>
                <option value="gold" style="background: #fa0;">Золотой</option>
                <option value="light-brown" style="background: #963; color: white;">Светло-коричневый</option>
                <option value="brown" style="background: #630; color: white;">Коричневый</option>
                <option value="dark-brown" style="background: #310; color: white;">Тёмно-коричневый</option>
                <option value="red" style="background: #f50; color: white;">Красный</option>
                <option value="blue" style="background: #09f; color: white;">Синий</option>
                <option value="green" style="background: #060; color: white;">Зелёный</option>
                <option value="purple" style="background: #c0c; color: white;">Фиолетовый</option>
                <option value="none">Нет</option>
            </select>
            <h3>Черты</h3>
                <textarea></textarea>
            <h3>Недостатки</h3>
                <textarea></textarea>
            <h3>Особенности</h3>
                <textarea></textarea>
            <h3>Предыстория</h3>
                <textarea></textarea>
        </div>
    </div>
</template>

<script>

export default {

    data: function () {
        return {
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
    },
    methods: {
    }
       
}
</script>

<style lang="scss">
</style>