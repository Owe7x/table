<template>
    <div>
        <div id="lvTable">
            <button class="button_table" @click="show = !show">TABLE</button>
            <button class="button_table" @click="show = !show">DDL</button>
            <div class="ddl" v-if="!show">
                <div class="generate" v-for="(item, index) in tbData" :key="index">
                    <span>INSER INTO categories</span>
                    <br>
                    (<span v-for="(item, index) in header" :key="index">
                        {{item.name}},
                    </span>)
                    <br>
                    VALUES
                    <br>
                    ({{index+1}} <span v-for="(item, index) in item" :key="index">
                        ,{{item}}
                    </span>);

                </div>
            </div>
            <div class="table" v-else>
                <table>
                    <thead>
                        <tr>
                            <th v-for="(name, index) in header" :key="index">
                                {{name.name}} 
                                <!-- <span v-for="(name,index) in name" :key="index">
                                    {{name}}
                                </span> -->
                                <button class="b20" v-if="header.length > 1" v-on:click="fDeleteColumn(index)" title="Удалить строку">X</button>
                            </th>
                            <th>X</th>
                        </tr>
                    </thead>
                    <tbody>
                <!-- Заполнение таблицы -->
                        <tr v-for="(entry, index) in tbData" :key="index">
                            <td style="text-align:center"><b>{{index + 1}}</b></td>
                            <td v-for="(name, index) in thData" :key="index">

                                <input type="text" v-model="entry[index ]" :key="index">
                            </td>
                            <td><button class="b20" v-if="tbData.length > 1" v-on:click="fDeleteRow(index)" title="Удалить строку">DELETE</button></td>
                        </tr>
                    </tbody>
                </table>
                <div class="footer">
                    <div class="footer_block">
                        <button v-on:click="fAddNewRow" title="Добавить строку в таблицу">Add Row</button>
                    </div>
                    <div class="footer_block">
                        <label for="">
                            Name<input type="text" v-model="addTh" /> 
                        </label>
                        <label for="">
                            Value   <select  v-model="innerValue" >
                                        <option >int</option>
                                        <option >varchar</option>
                                        <option >date</option>
                                        <option >datetime</option>
                                    </select>
                        </label>
                        <button @click="addColumn">Add Column</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            show: true,
            header: [
                {name: 'category_id', value: ''},
                {name: 'category_name', value: ''},
                {name: '3', value: ''},
                {name: '4', value: ''},
                {name: '5', value: ''},
                {name: '6', value: ''}
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
            addTh: "",
            newTbData: [
                {}
            ],
            counter: 0,
            innerValue: ''
        };
    },
    created() {
        console.log(this.thData);
        console.log(this.header);
    },
    methods: {
        addThData: function () {
        Object.assign(this.thData, { i: this.addTh });
        },
        addColumn: function () {
            this.header.push({name: this.addTh, value: this.innerValue})
            this.thData.push({name: this.addTh, value: ''})

            console.log('Элементов в строке', this.thData);
            console.log('Элементов в хэдере', this.header);
            
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
        fDeleteColumn: function (index) {
        // Удалить строку с номером index из таблицы
        this.header.splice(index, 1);
        this.thData.splice(index, 1)
        },
    },
};
</script>

<style lang="scss" scoped>

table {
    width: 630px;
    margin: auto;
    border: 2px solid #308090;
    border-radius: 3px;
    background-color: #fff;
}
thead {
    width: 100%;
}
#lvTable {
    width: 630px;
    margin: 20px 0px 0px 20px;
}
.button_table {
    padding: 5px 10px;
    border: 2px solid #308090;
    margin-bottom: 2px;
    margin-right: 2px;
}
.footer {
    margin-top: 20px;
    display: flex;
    &_block {
        border: 2px solid #308090;
        display: flex;
        align-items: center;
        margin-right: 20px;
        select {
            margin-right: 20px;
        }
        input {
            width: 100px;
            margin-left: 10px;
            margin-right: 20px;
            border: 2px solid  #308090;
            border-radius: 10px;
        }
        button {
            color: white;
            background-color: #308090;
            padding: 5px 10px;
            border: 1px solid white;
        }
    }
}
th,
td {
  /* Общие параметры заголовка и строк */
    min-width: 50px;
    padding: 10px 0px;
}
th {
    min-width: 140px;
    background-color: #308090;
    color: #ffffff;
    cursor: default;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
input {
    font-family: Helvetica Neue, Arial, sans-serif;
    font-size: 14px;
    color: #106070;

    width: 100%;
}
td {
    background-color: #f3f8f9;
    border: 1px solid black;
}
</style>