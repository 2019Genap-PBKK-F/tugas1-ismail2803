<template>
  <div>
    <div id="app" ref="spreadsheet"></div>
      <div><input type="button" value="Tambah Data Mahasiswa" @click="() => spreadsheet.insertRow()" /></div>
      <div><input type="button" value="Hapus Data Mahasiswa" @click="() => spreadsheet.deleteRow()" /></div>
  </div>
</template>

<script>
/* eslint-disable */ 
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'
import axios from 'axios'

export default {
  data(){
    return{
      biodata: [],
      form: {
        id: '',
        nrp: '',
        nama: '',
        angkatan: '',
        tgllahir: '',
        foto: '',
        aktif: ''
      }
    }
  },
  mounted() {
    let spreadsheet = jexcel(this.$el, this.Options)
    Object.assign(this, { spreadsheet })
  },
  methods: {
    tambahData(){
      axios.post('http://localhost:3000/biodata/', this.form).then(res => {
        console.log("Data baru", res.data)
      })
    },
    updateData(instance, cell, columns, row, value){
      axios.get('http://localhost:3000/biodata/').then(res => {
        var index = Object.values(res.data[row])
        index[columns] = value
        // console.log(index)
        axios.put('http://localhost:3000/biodata/' + index[0], {
          id: index[0],
          nrp: index[1],
          nama: index[2],
          angkatan: index[3],
          tgllahir: index[4],
          foto: index[5],
          aktif: index[6]
        }).then(res => {
          console.log("Update Data", res.data)
        })
      })
    },
    hapusData(instance, row){
      axios.get('http://localhost:3000/biodata/').then(res => {
        var index = Object.values(res.data[row])
        axios.delete('http://localhost:3000/biodata/' + index[0])
        console.log("Hapus Data", index)
      })
    }
  },
  computed: {
    Options(){
      return {
        data: this.biodata,
        allowToolbar: true,
        url: 'http://localhost:3000/biodata/',
        oninsertrow: this.tambahData,
        onchange: this.updateData,
        ondeleterow: this.hapusData,
        columns: [
          {title: 'id', width: '20px', type: 'hidden'},
          {title: 'NRP', width: '120px', type: 'text'},
          {title: 'Nama', width: '180px', type: 'text'},
          {title: 'Angkatan', width: '120px', type: 'numeric'},
          {title: 'Tanggal Lahir', width: '120px', type: 'calender', options: {format: 'DD/MM/YYY'}},
          {title: 'Photo', width: '180px', type: 'image'},
          {title: 'Aktif', width: '120px', type: 'checkbox'}
        ]
      }
    }
  }
}
</script>
