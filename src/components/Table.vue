<template>
  <div class="datatable">
    <table>
      <thead>
        <tr>
          <th class="row1 fixed">{{row1}}</th>
          <th class="row2 fixed">{{row2}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.index">
          <td class="row1">{{ item.staffID }}</td>
          <td class="row2">{{ item.staffLastName }}{{item.staffFirstName}}</td>
        </tr>
      </tbody>
    </table>
  </div>

</template>

<script>
export default {
    name:'table',
    props: [ 'row1', 'row2', 'items'],
   
}
</script>

<style scoped>
.datatable {
  overflow-y:scroll;
}
th {
    border: solid 1px;    
    padding: 10px;             /* 枠線指定 */
}
td {
    padding: 10px; 
    border-right: solid 1px;
}
table {
    border: solid 1px;
    border-collapse:  collapse; 
    width:  450px;
    height: 50px;      /* セルの線を重ねる */
}
.row1 {
    width: 30%
}
.row2 {
    width: 70%
}
.fixed {
  position: sticky;
  top: 0;
  color: #fff;
  background: #333;
}
.fixed::before{
  content: "";
    position: absolute;
    top: -1px;
    left: -1px;
    width: 100%;
    height: 100%;
    border: 1px solid #ccc;
}

</style>

