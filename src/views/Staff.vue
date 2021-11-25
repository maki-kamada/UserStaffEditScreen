<template>
  <div class="staff">
    <p>社員コード</p>
    
    <Table row1="社員コード" row2="社員名" :items= "items"/>

    <!-- <form @submit.prevent="post" > -->
    <!-- <form> -->
      <div id="registrations-app">
        <p class="title">社員コード</p>
        <FormInputNumber
          :placeholder="'111'"
          required
          v-model="code"
        />
        

        <p class="title">社員名</p>
        <div class="form-inline">
        <FormInputText  
         :placeholder="'山田'"
         required
         v-model="last_name"
         :name="'text'"
         :staff="staff"
         @update-status="updateStatus"/>
        <FormInputText 
         :placeholder="'太郎'" 
         required 
         v-model="first_name" 
         :name="'text'"
         :staff="staff"
         @update-status="updateStatus"
         />
        </div>
        
        <p class="title">社員名（カナ）</p>
        <div class="form-inline">
        <FormInputText  :placeholder="'ヤマダ'" required v-model="last_name_kana" :name="'kana'" />
        <FormInputText  :placeholder="'タロウ'" required v-model="first_name_kana" :name="'kana'"/>
        </div>

        <p class="title">郵便番号</p>
        <FormInputNumber required v-model="zipcode1"/>
        <FormInputNumber required v-model="zipcode2"/>

        <p class="title">住所1（都道府県）</p>
        <FormInputText required v-model="prefecture" :name="'text'"/>
        
        <p class="title">住所2（市区町村）</p>
        <FormInputText required v-model="city" :name="'text'"/>
        
        <p class="title">住所3（建物名等）</p>
        <FormInputText required = false v-model="building" :name="'text'"/>

        <p class="title">電話番号</p>
        <FormInputNumber required v-model="tel1" />
        <FormInputNumber required v-model="tel2" />
        <FormInputNumber required v-model="tel3" />

        <p class="title">メールアドレス</p>
        <FormInputText required v-model="mail" :name="'mail'"/>




        


        
      
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
        <button type="button" @click="post()" :disabled="!status">submit</button>
      </div>
    <!-- </form> -->
  </div>
</template>

<script>
import Table from '@/components/Table.vue'
import FormInputText from '@/components/FormInputText.vue'
import FormInputNumber from '@/components/FormInputNumber.vue'
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
    FormInputNumber,
    Table
  },
  data() {
    return{
      code: '',
      last_name: '',
      first_name: '',
      last_name_kana: '',
      first_name_kana: '',
      zipcode1: '',
      zipcode2: '',
      prefecture: '',
      city: '',
      building: '',
      tel1: '',
      tel2: '',
      tel3: '',
      mail: '',
      staff: {},

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
    updateStatus() {
        var _this = this
        _this.status = Object.keys(_this.staff).every(function (key) {
          return _this.staff[key];
        });
      }
  }

}


</script>
<style>

table {
  margin-left: auto;
  margin-right: auto;

}
</style>