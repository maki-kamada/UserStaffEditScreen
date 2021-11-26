<template>
  <div class="container">
    <div class="container">
      <table>
        <thead>
          <tr>
            <th class="row1 fixed">ID</th>
            <th class="row2 fixed">会社名</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.index">
            <td class="row1">{{ item.userID }}</td>
            <td class="row2">{{ item.companyName }}</td>
          </tr>
        </tbody>
      </table>
      <!-- <Table row1="ID" row2="会社名" :items="users"></Table> -->
      <button @click="showData" type="button">dataをshow!</button>
    </div>

    <form>
      <table>
        <div class="container">
          <!-- 会社名フォーム -->
          <tr>
            <div>
              <th><label for="company">会社名</label></th>
              <td><input type="text" v-model="company" /></td>
              <td class="errMsg">
                <span>{{ errors.company }}</span>
              </td>
            </div>
          </tr>
          <!-- 代表者名フォーム -->
          <tr>
            <div>
              <th><label for="president">代表者名</label></th>
              <td><input type="text" v-model="president" /></td>
              <td class="errMsg">
                <span>{{ errors.president }}</span>
              </td>
            </div>
          </tr>
          <!-- 郵便番号フォーム -->
          <tr>
            <div>
              <th><label for="zipcode">郵便番号</label></th>
              <td>
                <input class="numberInput" type="text" v-model="zipcode1" />
              </td>
              <td><span class="hyphen">―</span></td>
              <td>
                <input class="numberInput" type="text" v-model="zipcode2" />
              </td>
              <td class="errMsg">
                <span>{{ errors.zipcode }}</span>
              </td>
            </div>
          </tr>
          <!-- 住所１フォーム -->
          <tr>
            <div>
              <th><label for="prefecture">住所１(都道府県)</label></th>
              <td><input type="text" v-model="prefecture" /></td>
              <td class="errMsg">
                <span>{{ errors.prefecture }}</span>
              </td>
            </div>
          </tr>
          <!-- 住所２フォーム -->
          <tr>
            <div>
              <th><label for="city">住所２(市区町村)</label></th>
              <td><input type="text" v-model="city" /></td>
              <td class="errMsg">
                <span>{{ errors.city }}</span>
              </td>
            </div>
          </tr>
          <!-- 住所３フォーム -->
          <tr>
            <div>
              <th><label for="building">住所３(建物名等)</label></th>
              <td><input type="text" v-model="building" /></td>
              <td class="errMsg">
                <span>{{ errors.building }}</span>
              </td>
            </div>
          </tr>
          <!-- 電話番号フォーム -->
          <tr>
            <div class="aaa">
              <th><label for="tel">電話番号</label></th>
              <td><input class="numberInput" type="text" v-model="tel1" /></td>
              <td><span class="hyphen">―</span></td>
              <td><input class="numberInput" type="text" v-model="tel2" /></td>
              <td><span class="hyphen">―</span></td>
              <td><input class="numberInput" type="text" v-model="tel3" /></td>
              <td class="errMsg">
                <span>{{ errors.tel }}</span>
              </td>
            </div>
          </tr>
          <!-- FAXフォーム -->
          <tr>
            <div>
              <th><label for="fax">FAX</label></th>
              <td><input class="numberInput" type="text" v-model="fax1" /></td>
              <td><span class="hyphen">―</span></td>
              <td><input class="numberInput" type="text" v-model="fax2" /></td>
              <td><span class="hyphen">―</span></td>
              <td><input class="numberInput" type="text" v-model="fax3" /></td>
              <td class="errMsg">
                <span>{{ errors.fax }}</span>
              </td>
            </div>
          </tr>
          <!-- e-mailフォーム -->
          <tr>
            <div>
              <th><label for="mail">e-mail</label></th>
              <td><input type="text" v-model="mail" /></td>
              <td class="errMsg">
                <span>{{ errors.mail }}</span>
              </td>
            </div>
          </tr>
        </div>
      </table>
    </form>
    <div class="register">
      <button type="button" :disabled="isDisabled">送信</button>
    </div>
  </div>
</template>



<script>
// import Table from "@/components/Table.vue";

export default {
//   components: {
//     Table,
//   },
  data() {
    return {
      company: "",
      president: "",
      zipcode1: "",
      zipcode2: "",
      prefecture: "",
      city: "",
      building: "",
      tel1: "",
      tel2: "",
      tel3: "",
      fax1: "",
      fax2: "",
      fax3: "",
      mail: "",
      companyFlg: false,
      zipcodeFlg: false,
      prefectureFlg: false,
      cityFlg: false,
      telFlg: false,
      mailFlg: false,
      errors: {},
      items: null,
    };
  },
  methods: {
    showData() {
      this.axios
        .get("/api/Function1")
        .then((res) => {
          console.log(res.data);
          var string1 = JSON.stringify(res.data);
          let arr = JSON.parse(string1);
          console.log(arr);
          this.items = arr;
        })
        .catch((e) => {
          alert(e);
        });
    },
  },
  computed: {
    // 必須入力項目が未入力の場合、送信ボタンが非活性化する
    isDisabled() {
      return this.companyFlg == true &&
        this.zipcodeFlg == true &&
        this.prefectureFlg == true &&
        this.cityFlg == true &&
        this.telFlg == true &&
        this.mailFlg == true &&
        Object.keys(this.errors).length == 0
        ? false
        : true;
    },
  },
  watch: {
    // リアルタイムでバリデーションチェックを行う
    company(company) {
      if (!company) {
        this.$set(this.errors, "company", "必須入力項目です。");
        this.companyFlg = false;
      } else if (company.length > 50) {
        this.$set(this.errors, "company", "50文字以内で入力してください。");
        this.companyFlg = false;
      } else {
        this.$delete(this.errors, "company");
        this.companyFlg = true;
      }
    },
    president(president) {
      if (president.length > 50) {
        this.$set(this.errors, "president", "50文字以内で入力してください。");
      } else {
        this.$delete(this.errors, "president");
      }
    },
    zipcode1(zipcode1) {
      if (!zipcode1 || !this.zipcode2) {
        this.$set(this.errors, "zipcode", "必須入力項目です。");
        this.zipcodeFlg = false;
      } else if (zipcode1.match(/^\d{3}$/) && this.zipcode2.match(/^\d{4}$/)) {
        this.$delete(this.errors, "zipcode");
        this.zipcodeFlg = true;
      } else {
        this.$set(
          this.errors,
          "zipcode",
          "半角数字で入力してください。（3桁＋4桁）"
        );
        this.zipcodeFlg = false;
      }
    },
    zipcode2(zipcode2) {
      if (!this.zipcode1 || !zipcode2) {
        this.$set(this.errors, "zipcode", "必須入力項目です。");
        this.zipcodeFlg = false;
      } else if (this.zipcode1.match(/^\d{3}$/) && zipcode2.match(/^\d{4}$/)) {
        this.$delete(this.errors, "zipcode");
        this.zipcodeFlg = true;
      } else {
        this.$set(
          this.errors,
          "zipcode",
          "半角数字で入力してください。（3桁＋4桁）"
        );
        this.zipcodeFlg = false;
      }
    },
    prefecture(prefecture) {
      if (!prefecture) {
        this.$set(this.errors, "prefecture", "必須入力項目です。");
        this.prefectureFlg = false;
      } else if (prefecture.length > 50) {
        this.$set(this.errors, "president", "50文字以内で入力してください。");
        this.prefectureFlg = false;
      } else {
        this.$delete(this.errors, "prefecture");
        this.prefectureFlg = true;
      }
    },
    city(city) {
      if (!city) {
        this.$set(this.errors, "city", "必須入力項目です。");
        this.cityFlg = false;
      } else if (city.length > 50) {
        this.$set(this.errors, "city", "50文字以内で入力してください。");
        this.cityFlg = false;
      } else {
        this.$delete(this.errors, "city");
        this.cityFlg = true;
      }
    },
    building(building) {
      if (building.length > 50) {
        this.$set(this.errors, "building", "50文字以内で入力してください。");
      } else {
        this.$delete(this.errors, "building");
      }
    },
    tel1(tel1) {
      if (!tel1 || !this.tel2 || !this.tel3) {
        this.$set(this.errors, "tel", "必須入力項目です。");
        this.telFlg = false;
      } else if (
        tel1.match(/^\d{2,3}$/) &&
        this.tel2.match(/^\d{1,4}$/) &&
        this.tel3.match(/^\d{4}$/)
      ) {
        this.$delete(this.errors, "tel");
        this.telFlg = true;
      } else {
        this.$set(
          this.errors,
          "tel",
          "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
        );
        this.telFlg = false;
      }
    },
    tel2(tel2) {
      if (!this.tel1 || !tel2 || !this.tel3) {
        this.$set(this.errors, "tel", "必須入力項目です。");
        this.telFlg = false;
      } else if (
        this.tel1.match(/^\d{2,3}$/) &&
        tel2.match(/^\d{1,4}$/) &&
        this.tel3.match(/^\d{4}$/)
      ) {
        this.$delete(this.errors, "tel");
        this.telFlg = true;
      } else {
        this.$set(
          this.errors,
          "tel",
          "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
        );
        this.telFlg = false;
      }
    },
    tel3(tel3) {
      if (!this.tel1 || !this.tel2 || !tel3) {
        this.$set(this.errors, "tel", "必須入力項目です。");
        this.telFlg = false;
      } else if (
        this.tel1.match(/^\d{2,3}$/) &&
        this.tel2.match(/^\d{1,4}$/) &&
        tel3.match(/^\d{4}$/)
      ) {
        this.$delete(this.errors, "tel");
        this.telFlg = true;
      } else {
        this.$set(
          this.errors,
          "tel",
          "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
        );
        this.telFlg = false;
      }
    },
    fax1(fax1) {
      if (fax1 == "" && this.fax2 == "" && this.fax3 == "") {
        this.$delete(this.errors, "fax");
      } else if (
        !fax1.match(/^\d{2,3}$/) ||
        !this.fax2.match(/^\d{1,4}$/) ||
        !this.fax3.match(/^\d{4}$/)
      ) {
        this.$set(
          this.errors,
          "fax",
          "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
        );
      } else {
        this.$delete(this.errors, "fax");
      }
    },
    fax2(fax2) {
      if (this.fax1 == "" && fax2 == "" && this.fax3 == "") {
        this.$delete(this.errors, "fax");
      } else if (
        !this.fax1.match(/^\d{2,3}$/) ||
        fax2.match(/^\d{1,4}$/) ||
        !this.fax3.match(/^\d{4}$/)
      ) {
        this.$set(
          this.errors,
          "fax",
          "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
        );
      } else {
        this.$delete(this.errors, "fax");
      }
    },
    fax3(fax3) {
      if (this.fax1 == "" && this.fax2 == "" && fax3 == "") {
        this.$delete(this.errors, "fax");
      } else if (
        !this.fax1.match(/^\d{2,3}$/) ||
        !this.fax2.match(/^\d{1,4}$/) ||
        !fax3.match(/^\d{4}$/)
      ) {
        this.$set(
          this.errors,
          "fax",
          "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
        );
      } else {
        this.$delete(this.errors, "fax");
      }
    },
    mail(mail) {
      if (!mail) {
        this.$set(this.errors, "mail", "必須入力項目です。");
        this.mailFlg = false;
      } else if (
        !mail.match(
          /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
        )
      ) {
        this.$set(this.errors, "mail", "正しいメール形式で入力してください。");
        this.mailFlg = false;
      } else if (mail.length > 40) {
        this.$set(this.errors, "mail", "40文字以内で入力してください。");
        this.mailFlg = false;
      } else {
        this.$delete(this.errors, "mail");
        this.mailFlg = true;
      }
    },
  },
};
</script>



<style scoped>
.container {
  width: 380px;
  margin: 50px auto;
}
label {
  display: inline-block;
  white-space: nowrap;
}
input {
  width: 300px;
  font-size: 20px;
  padding: 0px;
  margin-bottom: 15px;
}
button {
  width: 60px;
  height: 40px;
}
.register {
  width: 100%;
  text-align: center;
}
span {
  font-size: 14px;
  color: red;
  width: 90%;
  display: block;
}
.hyphen {
  width: 30px;
}
th {
  display: block;
  text-align: left;
  margin: 0px;
}
.numberInput {
  width: 100px;
}
.errMsg {
  display: flex;
  vertical-align: bottom;
  text-align: left;
}
</style>