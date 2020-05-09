<template>
  <div>
    <div id="app" ref="spreadsheet"></div>
    <div>
        <input class="btn btn-primary tambah" type="button" value="Add New Row" @click="() => spreadsheet.insertRow()" />
        <input class="btn btn-primary tambah" type="button" value="Delete Selected Row" @click="() => spreadsheet.deleteRow()" />
    </div>
  </div>
</template>

<script>
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'
import axios from 'axios'
var host = 'http://10.199.14.46:8030/'
// var host = 'http://localhost:8030/'
var dropdownJenisSatker = 'http://10.199.14.46:8030/api/jenissatker/nama/'
var dropdownSatuanKerja = 'http://10.199.14.46:8030/api/satuankerja/nama/'
// var dropdownJenisSatker = 'http://localhost:8030/api/jenissatker/nama/'
// var dropdownSatuanKerja = 'http://localhost:8030/api/satuankerja/nama/'
export default {
  // name: 'App',
  data() {
    return {
      masterIndikator: [],
      form: {
        id: 'aff',
        id_jns_satker: 1,
        id_induk_satker: 'aff',
        nama: 'New Data'
      }
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load() {
      axios.get(host + 'api/satuankerja/').then(res => {
        console.log(res.data)
        var jexcelOptions = {
          data: res.data,
          allowToolbar: true,
          onchange: this.updateRow,
          oninsertrow: this.newRow,
          ondeleterow: this.deleteRow,
          responsive: true,
          columns: [
            { type: 'text', title: 'id', width: '50px' },
            { type: 'dropdown', title: 'Jenis Satker', url: dropdownJenisSatker, width: '120px' },
            { type: 'dropdown', title: 'Induk Satker', url: dropdownSatuanKerja, width: '120px' },
            { type: 'text', title: 'Nama', width: '120px' },
            { type: 'text', title: 'Email', width: '120px' },
            { type: 'text', title: 'level_unit', width: '120px' },
            { type: 'text', title: 'Create Date', width: '150px', readOnly: true },
            { type: 'text', title: 'Last Update', width: '150px', readOnly: true },
            { type: 'text', title: 'Expired Date', width: '150px' }
          ]
        }
        let spreadsheet = jexcel(this.$el, jexcelOptions)
        Object.assign(this, { spreadsheet })
      })
    },
    newRow() {
      axios.post(host + 'api/satuankerja/', this.form).then(res => {
        console.log(res.data)
      })
    },
    updateRow(instance, cell, columns, row, value) {
      axios.get(host + 'api/satuankerja/').then(res => {
        var index = Object.values(res.data[row])
        index[columns] = value
        console.log(index)
        axios.put(host + 'api/satuankerja/' + index[0], {
          id: index[0],
          id_jns_satker: index[1],
          id_induk_satker: index[2],
          nama: index[3],
          email: index[4],
          level_unit: index[5]
        }).then(res => {
          console.log(res.data)
        })
      })
    },
    deleteRow(instance, row) {
      axios.get(host + 'api/satuankerja/').then(res => {
        var index = Object.values(res.data[row])
        // console.log(index)
        console.log(row)
        axios.delete(host + 'api/satuankerja/' + index[0])
      })
    }
  }
}
</script>
<style>
  .tambah {
    margin-top: 10pt;
    margin-bottom: 10pt;
    margin-left: 10pt;
    }
</style>
