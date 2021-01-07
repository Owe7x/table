<template>
  <div id="app">
    <div class="header-button">
        <div class="toggle">
            <button class="button_table" @click="Table()">TABLE</button>
            <button class="button_table" @click="Ddl()">DDL</button>
            <button class="button_table" @click="Change()" title="Редактировать value">Редактировать</button>
        </div>
    </div>
    <div class="header-change" >
      <div class="header-modal" v-show="change" v-for="(item, index) in headers" :key="index">
          <label >
          Name: 
          </label>
          <input type="text" v-model="item.headerName">
          <label for="" >
          Value:   <select  v-model="item.headerType" >
                      <option >int</option>
                      <option >varchar</option>
                      <option >date</option>
                      <option >datetime</option>
                  </select>
          </label>
      </div>

    </div>
    <div class="ddl"  v-show="ddl">
      <div class="generate-table">
          <span>CREATE TABLE categories</span>
          <br>
          (<span v-for="(items, index) in headers" :key="index">
              {{items.headerName}} {{items.headerType}} NOT NULL, <br>
          </span>)
          
      </div>
      <div class="generate" v-for="(item, index) in products" :key="index">
          <span>INSER INTO categories</span>
          <br>
          (<span v-for="(items, index) in headers" :key="index">
                  {{items.headerName}},
              </span> );
          <br>
          VALUES
          <br>
          <div>
              ({{index+1}} <span v-for="(item, index) in item" :key="index">
                        {{item.value}},
                    </span>);
          </div>
      </div>
    </div>
    
    <div class="table" v-show="table">
      <vue-table
      :tbody-data="products"
      :headers="headers"
      :custom-options="customOptions"
      :style-wrap-vue-table="styleWrapVueTable"
      :disable-cells="disableCells"
      :disable-sort-thead="disableSortThead"
      :loading="loading"
      :parent-scroll-element="parentScrollElement"
      :select-position="selectPosition"
      :submenu-tbody="submenuTbody"
      :submenu-thead="submenuThead"
      v-on:tbody-change-data="changeData"
      v-on:tbody-submenu-click-change-color="changeColorTbody"
      v-on:tbody-submenu-click-change-value="changeValueTbody"
      v-on:thead-submenu-click-change-color="changeColorThead"
      v-on:thead-submenu-click-change-value="changeValueThead"
      v-on:thead-td-sort="sortProduct"
      >
    <div slot="loader">
      Loader
    </div>
    </vue-table>
    <button @click="show = !show" class="button">New Column</button>
    <button @click="AddRow()" class="button">New Row</button>
    <div class="modal-show" v-if="show">
      <div @click="show = !show" class="modal-close">X</div>
      <div class="show-input">Name <input v-model="headerName"></div>
      <div class="show-input">Key <input v-model="headerKey"></div>
      <div class="show-input">Type:  <label for="" >
          <select  v-model="headerType" >
                      <option >int</option>
                      <option >varchar</option>
                      <option >date</option>
                      <option >datetime</option>
                  </select>
          </label></div>
      <button @click="AddColumn(); show = !show" class="show-button">New Column</button>
    </div>
    </div>
    


  </div>
</template>

<script>

import VueTable from 'vuejs-spreadsheet';

export default {
  name: 'app',
  data() {
    return {
      table: true,
      ddl: false,
      change: false,
      headerName: "",
      headerType: "",
      headerKey: "",
      show: false,
      customOptions: {
        tbodyIndex: true,
        sortHeader: true,
        trad: {
          lang: 'fr',
          en: {
            select: {
              placeholder: 'Search by typing',
            },
          },
          fr: {
            select: {
              placeholder: 'Taper pour chercher',
            },
          },
        },
        newData: {
          type: 'input',
          value: '',
          active: false,
          style: {
            color: '#000',
          },
        },
        fuseOptions: {
          shouldSort: true,
          threshold: 0.2,
          location: 0,
          distance: 30,
          maxPatternLength: 64,
          minMatchCharLength: 1,
          findAllMatches: false,
          tokenize: false,
          keys: [
            'value',
          ],
        },
      },
      submenuTbody: [
        {
          type: 'button',
          value: 'change color',
          function: 'change-color',
          disabled: ['img'],
        },
        {
          type: 'button',
          value: 'change value',
          function: 'change-value',
          disabled: ['img', 'name'],
        },
      ],
      submenuThead: [
        {
          type: 'button',
          value: 'change color',
          function: 'change-color',
          disabled: ['a'],
        },
        {
          type: 'select',
          disabled: ['a'],
          subtitle: 'Select state:',
          selectOptions: [
            {
              value: 'new-york',
              label: 'new-york',
            },
            {
              value: 'france',
              label: 'france',
            },
          ],
          value: 'new-york',
          buttonOption: {
            value: 'change city',
            function: 'change-city',
            style: {
              display: 'block',
            },
          },
        },
        {
          type: 'button',
          value: 'change value',
          function: 'change-value',
          disabled: ['a', 'b'],
        },
      ],
      disableCells: ['a'],
      loading: false,
      parentScrollElement: {
        attribute: 'html',
        positionTop: 0,
      },
      selectPosition: {
        top: 0,
        left: 0,
      },
      disableSortThead: ['a'],
      styleWrapVueTable: {
        fontSize: '12px',
        comment: {
          borderColor: '#696969',
          borderSize: '8px',
          widthBox: '120px',
          heightBox: '80px',
        },
      },
      headers: [
        {
          headerName: 'A',
          headerKey: 'a',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
        },
        {
          headerName: 'B',
          headerKey: 'b',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
        },
        {
          headerName: 'C',
          headerKey: 'c',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
        },
        {
          headerName: 'D',
          headerKey: 'd',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
        },
        {
          headerName: 'E',
          headerKey: 'e',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
        },
        {
          headerName: 'F',
          headerKey: 'f',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
        },
        {
          headerName: 'G',
          headerKey: 'g',
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
        },
      ],
      products: [
        {
          a: {
            type: 'img',
            value: 'https://via.placeholder.com/350x150',
            active: false,
          },
          c: {
            type: 'input',
            value: 'Paris',
            active: false,
            style: {
              color: '#000',
            },
          },
          d: {
            type: 'input',
            value: 'France',
            active: false,
            style: {
              color: '#000',
            },
          },
          e: {
            type: 'input',
            value: 'Boe',
            active: false,
            style: {
              color: '#000',
            },
          },
          f: {
            type: 'select',
            handleSearch: true,
            selectOptions: [
              {
                value: 'Harry Potter',
                label: 'harry potter',
              },
              {
                value: 'Hermione Granger',
                label: 'hermione granger',
              },
              {
                value: 'Ron Whisley',
                label: 'ron whisley',
              },
              {
                value: 'Dobby',
                label: 'dobby',
              },
              {
                value: 'Hagrid',
                label: 'hagrid',
              },
              {
                value: 'Professeur Rogue',
                label: 'professeur rogue',
              },
              {
                value: 'Professeur Mcgonagal',
                label: 'professeur mcgonagal',
              },
              {
                value: 'Professeur Dumbledor',
                label: 'professeur dumbledor',
              },
            ],
            value: 'professeur dumbledor',
            active: false,
          },
          g: {
            type: 'select',
            handleSearch: true,
            selectOptions: [
              {
                value: '1980',
                label: '1980',
              },
              {
                value: 1981,
                label: 1981,
              },
              {
                value: 1982,
                label: 1982,
              },
              {
                value: 1983,
                label: 1983,
                active: true,
              },
              {
                value: 1984,
                label: 1984,
              },
            ],
            value: 1983,
            active: false,
          },
        },
      ],
    };
  },
  components: {
    VueTable,
  },
  mounted() {
    this.loading = true;
    setTimeout(() => {
      this.loading = false;
    }, 300);
  },
  methods: {
    changeData(row, header) {
      console.log(row, header);
    },
    sortProduct() {
      console.log('sort product');
    },
    // callback
    changeColorThead(event, header, colIndex) {
      this.headers[colIndex].style.color = '#e40000';
    },
    changeColorTbody(event, header, rowIndex) {
      this.products[rowIndex][header].style = {};
      this.products[rowIndex][header].style.color = '#e40000';
    },
    changeValueTbody(event, header, rowIndex) {
      this.products[rowIndex][header].value = 'T-shirt';
    },
    changeValueThead(event, entry, colIndex) {
      this.headers[colIndex].headerName = this.headerName;
    },
    AddColumn() {
      this.headers.push({
          headerName: this.headerName,
          headerKey: this.headerKey,
          headerType: this.headerType,
          style: {
            width: '100px',
            minWidth: '100px',
            color: '#000',
          },
      })
    },
    AddRow() {
      this.products.push({
          a: {
            type: 'img',
            value: 'https://via.placeholder.com/350x150',
            active: false,
          },
          c: {
            type: 'input',
            value: 'Paris',
            active: false,
            style: {
              color: '#000',
            },
          },
          d: {
            type: 'input',
            value: 'France',
            active: false,
            style: {
              color: '#000',
            },
          },
          e: {
            type: 'input',
            value: 'Boe',
            active: false,
            style: {
              color: '#000',
            },
          },
          f: {
            type: 'select',
            handleSearch: true,
            selectOptions: [
              {
                value: 'Harry Potter',
                label: 'harry potter',
              },
              {
                value: 'Hermione Granger',
                label: 'hermione granger',
              },
              {
                value: 'Ron Whisley',
                label: 'ron whisley',
              },
              {
                value: 'Dobby',
                label: 'dobby',
              },
              {
                value: 'Hagrid',
                label: 'hagrid',
              },
              {
                value: 'Professeur Rogue',
                label: 'professeur rogue',
              },
              {
                value: 'Professeur Mcgonagal',
                label: 'professeur mcgonagal',
              },
              {
                value: 'Professeur Dumbledor',
                label: 'professeur dumbledor',
              },
            ],
            value: 'professeur dumbledor',
            active: false,
          },
          g: {
            type: 'select',
            handleSearch: true,
            selectOptions: [
              {
                value: 1980,
                label: 1980,
              },
              {
                value: 1981,
                label: 1981,
              },
              {
                value: 1982,
                label: 1982,
              },
              {
                value: 1983,
                label: 1983,
                active: true,
              },
              {
                value: 1984,
                label: 1984,
              },
            ],
            value: 1983,
            active: false,
          },
        })
    },
    Table() {
      this.table = true
      this.ddl = false
      this.change = false
    },
    Ddl() {
      this.table = false
      this.ddl = true
      this.change = false
    },
    Change() {
      if(this.ddl) {
        this.change = false
      } else if (this.change) {
        this.change = false
      } else {
        this.change = true
      }
    }

  },
};
</script>

<style lang="scss">
#app {
  padding: 20px;
}
::-moz-selection {
  color: #2c3e50;
  background: transparent;
}
::selection {
  color: #2c3e50;
  background: transparent;
}
.button {
  margin-top: 10px;
  margin-right: 10px;
  background-color: #e7ecf5;
  padding: 10px 15px;
}
.modal-show {
  display: block;
  position: fixed;
  top: 50%;
  left: 45%;
  width: 300px;
  height: 200px;
  padding: 20px;
  background-color: #e7ecf5;
  display: -webkit-box;
  display: -ms-flexbox;
  text-align: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
          justify-content: center;
  .show-button {
    border: 1px solid black;
    padding: 5px 10px;
  }
  .modal-close {
    text-align: right;
    margin-bottom: 10px;
    cursor: pointer;
  }
}
.show-input {
  display: flex;
  margin-bottom: 10px;
  font-weight: bold;
  input {
    border: 1px solid black;
    margin-left: 5px;
  }
}
.generate-table {
    padding: 10px;
    background-color: #e6ecf6;
    border-left: 3px solid #478bf8;
    margin-bottom: 10px;
    text-align: center;
}
.generate { 
    padding: 10px;
    background-color: #e6ecf6;
    border-left: 3px solid #478bf8;
}
.button_table {
    padding: 5px 10px;
    border: 2px solid #e6ecf6;
    margin-bottom: 2px;
    margin-right: 2px;
    &:hover {
        border: 2px solid #9fa3aa;
        background: #e6ecf6;
    }
    &:focus {
        outline: none
    }
}
.header-change {
  display: flex;
  margin-bottom: 10px;
  margin-left: 28px;
}
.header-modal { 
  width: 99px;
  min-width: 99px;
  color: #000;
  background-color: #e6ecf6;
  margin-right: 1px;
  padding: 5px;
  font-size: 12px;
  input {
    width: 50px;
  }
}
</style>