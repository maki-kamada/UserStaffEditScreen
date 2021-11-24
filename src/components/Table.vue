<template>
  <div>
    <table>
      <thead>
        <tr>
          <th class="row1">{{row1}}</th>
          <th class="row2">{{row2}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.index">
          <td class="row1">{{ item.code }}</td>
          <td class="row2">{{ item.last_name }}</td>
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
    width:  50%;      /* セルの線を重ねる */
}
.row1 {
    width: 20%
}
.row2 {
    width: 80%
}

</style>

