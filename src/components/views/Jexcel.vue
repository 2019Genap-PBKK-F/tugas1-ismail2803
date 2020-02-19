<template>
  <section class="content">
    <div>
      <div id="app" ref="spreadsheet"></div>
      <div><input type="button" value="Tambah Data Mahasiswa" @click="() => spreadsheet.insertRow()" /></div>
      <input type="button" value="Hapus Data Mahasiswa" @click="() => spreadsheet.deleteRow()" />
    </div>
  </section>
</template>

<script>
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'
var data = [
  ['05111740007007', 'Ismail Arifin', '2017', '28/03/1999', '', true]
]
var options = {
  data: data,
  allowToolbar: true,
  columns: [
    {title: 'NRP', width: '120px', type: 'text'},
    {title: 'Nama', width: '180px', type: 'text'},
    {title: 'Angkatan', width: '120px', type: 'numeric'},
    {title: 'Tanggal Lahir', width: '120px', type: 'calender', options: {format: 'DD/MM/YYY'}},
    {title: 'Photo', width: '180px', type: 'image'},
    {title: 'Aktif', width: '120px', type: 'checkbox'}
  ]
}
export default {
  name: 'App',
  mounted: function () {
    let spreadsheet = jexcel(this.$el, options)
    Object.assign(this, { spreadsheet })
  }
}
</script>
