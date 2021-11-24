<template>
  <div class="staff">
    <p>社員コード</p>
    
    <Table row1="社員コード" row2="社員名" :items= "items"/>

    <!-- <form @submit.prevent="post" > -->
    <!-- <form> -->
      <div id="registrations-app">
        <p class="title">社員コード</p>
        <FormInputText   :placeholder="'111'" :name="'number'" />

        

        <p class="title">社員名</p>
        <div class="form-inline">
        <FormInputText  :placeholder="'山田'" :name="'number'" />
        <FormInputText  :placeholder="'太郎'" :name="number" />
        </div>
        
        <p class="title">社員名（カナ）</p>
        <div class="form-inline">
        <FormInputText  :placeholder="'ヤマダ'" :name="number" />
        <FormInputText  :placeholder="'タロウ'" :name="number" />
        </div>
        <input v-model="code" />
        <input v-model="last_name" />
        
      
        <!--<p class="title">郵便番号</p>
        
        <input v-model="zipcode1" />
        <input v-model="zipcode2" />
        <div class="result">
          {{ zipcode1 }}<br />
          {{ zipcode2 }}
        </div>

        <p class="title">住所1（都道府県）</p>
        <input v-model="prefecture" />
        <div class="result">
          {{ prefecture }}
        </div>

        <p class="title">住所2（市区町村）</p>
        <input v-model="city" />
        <div class="result">
          {{ city }}
        </div>

        <p class="title">住所3（建物名等）</p>
        <input v-model="building" />
        <div class="result">
          {{ building }}
        </div>

        <p class="title">電話番号</p>
        <input v-model="tel1" />
        <span> - </span>
        <input v-model="tel2" />
        <span> - </span>
        <input v-model="tel3" />

        <p class="title">メールアドレス</p>
        <input v-model="mail" />
        <div class="result">入力：{{ mail }}</div>
        <button type="button" @click="post()">submit</button> -->
        <button type="button" @click="post()">submit</button>
      </div>
    <!-- </form> -->
  </div>
</template>

<script>
import Table from '@/components/Table.vue'
import FormInputText from '@/components/FormInputText.vue'
// import Vue from 'vue'

// Vue.component('FormInputText', FormInputText)

// $(function () {
//   new Vue({
//     el: '#registrations-app',
//     data: {
//       staff: {
//         type: Object
//       }
//     }
//   });
// });
export default {
  name: 'staff',
  components: {
    FormInputText,
    Table
  },
  data() {
    return{
      staff: {
        type: Object
      },
      items: [
        { code: 100, last_name: "aaa" },
        { code: 200, last_name: "bbb" },
      ],
    }
  },
  methods: {
    post() {
      this.items.push({ code: this.code, last_name: this.last_name });
    },
  }

}


</script>
<style>

table {
  margin-left: auto;
  margin-right: auto;

}
</style>