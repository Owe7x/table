<template>
    <div id="lvTable">
        <table>
            <thead>
                <tr>
                    <th v-for="(name, index) in header" :key="index">
                        {{name}}
                    </th>
                </tr>
            </thead>
        </table>
        <tbody>
        <!-- Заполнение таблицы -->
            <tr v-for="(entry, index) in tbData" :key="index">
                {{entry}}
                <td style="text-align:center"><b>{{index + 1}}</b></td>
                <td v-for="(name, index) in thData" :key="index">
                    {{name}} 
                    <input type="text" v-model="entry['id' + index ]" :key="index">
                </td>

            </tr>
        </tbody>
        <li v-for="(entry, index) in tbData" :key="index">
            {{entry[0].id++}}
        </li>
        
        <tfoot>
        <!-- футер таблицы -->

            <button class="b20" v-on:click="fAddNewRow" title="Добавить строку в таблицу">Добавить строку</button>
            <!-- Кнопка добавления стороки в таблицу -->
            
            <!-- Итоговая сумма -->

        </tfoot>
        <input type="text" v-model="addTh" />
        <button @click="addColumn">Добавить колонку</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            header: [
                '1',
                '2',
                '3',
                '4',
                '5'
            ],
            tbFullPrice: 0,
            tbData: [
                // Массив строк
                {  },

            ],
            thData: [
                {name: '1', value: ''},
                {name: '2', value: ''},
                {name: '3', value: ''},
                {name: '4', value: ''},
                {name: '5', value: ''},
            ],
            addTh: "1",
        };
    },
    created() {
        console.log(this.thData);
    },
    methods: {
        addThData: function () {
        Object.assign(this.thData, { i: this.addTh });
        },
        addColumn: function () {
            this.header.push(this.addTh)
            this.thData.push({name: this.addTh, value: ''})

            console.log('Элементов в строке', this.thData);
            
        },
        fAddNewRow: function () {
        // Добавить новую строку в таблицу
        this.tbData.push({ });
        console.log('Колличество строк',this.tbData);
        },
        fDeleteRow: function (index) {
        // Удалить строку с номером index из таблицы
        this.tbData.splice(index, 1);
        },
    },
};
</script>

<style lang="scss" scoped>
.headers {
    min-width: 300px;
    background-color: rgb(102, 102, 102);
    margin: 0 auto;
    display: flex;
    justify-content: center;
    &__elem {
        color: white;
        margin-right: 20px;
    }
}
th {
    width: 80px;
    padding: 20px;
    background-color: grey;
    border: 1px solid black;
}
input {
    border: 1px solid black;
}
td {
    width: 80px;
    padding: 20px;
    background-color: rgb(136, 28, 28);
    border: 1px solid black;
}
</style>