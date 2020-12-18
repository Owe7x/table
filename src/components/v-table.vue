<template>
    <div id="lvTable">
  <table>
    <thead>
      <!-- Заголовок таблицы -->
      <tr>
        <th>№</th>
        <th>Наименование</th>
        <th>Количество</th>
        <th class="th60">Цена</th>
        <th class="th60">Сумма</th>
        <th>Х</th>
      </tr>
    </thead>
    <tbody>
      <!-- Заполнение таблицы -->
      <tr v-for="(entry, index) in tbData" :key="index">
        {{entry}}
        <!-- Цикл по строкам -->
        <td style="text-align:center"><b>{{index + 1}}</b></td>
        <!-- Номер строки -->
        <td><input class="th180"  v-model="entry['name']" placeholder="Введите наименование" title="Введите наименование товара"></td>
        <!-- Наименование -->
        <td><input class="th60" v-model="entry['count']" type="number" min="0" max="999999" maxlength="6" title="Введите количество товара"></td>
        <!-- Количество -->
        <td><input class="th60"  v-model="entry['price']" type="number" min="0" max="999999" maxlength="6" title="Введите цену товара"></td>
        <!-- Цена -->
        <td><input class="th60 thAm" v-model="entry['amount']" readonly title="Сумма за товар"></td>
        <!-- Сумма -->
        <td><input class="th60"  v-model="entry['price2']" type="number" min="0" max="999999" maxlength="6" title="Введите цену товара"></td>

        <td><button class="b20" v-if="tbData.length > 1" v-on:click="fDeleteRow(index)" title="Удалить строку">-</button></td>
        <!-- Удалить строку (если строк больше одной) -->
      </tr>
    </tbody>
    <tfoot>
      <!-- футер таблицы -->
      <tr>
        <td><button class="b20" v-on:click.prevent="fAddNewRow" title="Добавить строку в таблицу">+</button></td>
        <!-- Кнопка добавления стороки в таблицу -->
        <td colspan="3" style="text-align:right"><b>ИТОГО:</b></td>

        <!-- Итоговая сумма -->
      </tr>
    </tfoot>
  </table>
  {{tbData[0].name}}

</div>
</template>

<script>
export default {
    data() {
        return {
            tbFullPrice: 0,
            tbData: [ // Массив строк
                { name: '', count: 1, price: 1, amount: 1, price2: 1}
            ],
            thData: {
                name: '',
                count: 1,
                price: 1,
                amount: 1,
            },
            addTh: '11'

        }
    },
    created() {
        console.log(this.thData);
    },
    methods: {
        addThData: function() {
            Object.assign(this.thData, {i : this.addTh})
        },
        addColumn: function() {
                this.addThData()
        },
        fAddNewRow: function () { // Добавить новую строку в таблицу
        
        this.tbData.push({ name: '', count: 1, price: 1, amount: 1, price2: 1 });
        },
        fDeleteRow: function (index) { // Удалить строку с номером index из таблицы
        this.tbData.splice(index, 1);
        }
    }
}
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

</style>