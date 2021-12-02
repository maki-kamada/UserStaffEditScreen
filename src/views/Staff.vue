<template>
  <div class="container">
    <p>社員マスタ</p>
    <div class="item datatable">
      <table class="data">
        <thead>
          <tr>
            <th class="row1 fixed">社員コード</th>
            <th class="row2 fixed">社員名</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="item in items"
            :key="item.staffID"
            @click="selectRow(item)"
            :class="{ highlight: item == selectedStaff }"
          >
            <td class="row1">{{ item.staffID }}</td>
            <td class="row2">
              {{ item.staffLastName }} {{ item.staffFirstName }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <!-- <Table row1="社員コード" row2="社員名" :items= "staffs" ></Table> -->
    <!-- <button @click="getIp" type="button">IPをGET!</button> -->
    <div class="item">
      <form>
        <table class="form">
          <!-- 社員コードフォーム -->
          <tr>
            <div>
              <th><label for="code">社員コード</label></th>
              <td><input type="text" v-model="code" /></td>
              <td class="errMsg">
                <span>{{ errors.code }}</span>
              </td>
            </div>
          </tr>
          <!-- 社員名フォーム -->
          <tr>
            <div>
              <th><label for="name">社員名</label></th>
              <td>
                <input class="inputdev" type="text" v-model="last_name" />
              </td>
              <td>
                <input class="inputdev" type="text" v-model="first_name" />
              </td>
              <td class="errMsg">
                <span>{{ errors.name }}</span>
              </td>
            </div>
          </tr>

          <!-- 社員名（カナ）フォーム -->
          <tr>
            <div>
              <th><label for="namekana">社員名（カナ）</label></th>
              <td>
                <input class="inputdev" type="text" v-model="last_name_kana" />
              </td>
              <td>
                <input class="inputdev" type="text" v-model="first_name_kana" />
              </td>
              <td class="errMsg">
                <span>{{ errors.namekana }}</span>
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
        </table>
      </form>

      <div class="register">
        <button type="button" @click="clear">クリア</button>
        <button type="button" :disabled="isDisabled" @click="post">
          新規追加
        </button>
        <button type="button" :disabled="editDisabled" @click="put">更新</button>
        <button type="button" :disabled="editDisabled" @click="del">削除</button>
      </div>
    </div>
  </div>
</template>



<script>
// import Table from '@/components/Table.vue'

export default {
  // components: {
  //     Table
  // },
  beforeCreate() {
    this.axios
      .get("/api/StaffListFunction/")
      .then((res) => {
        console.log(res.data);
        var string1 = JSON.stringify(res.data);
        let arr = JSON.parse(string1);
        console.log(arr);
        this.items = arr;
        this.items.sort((a, b) => {
          return a.staffID - b.staffID;
        });
      })
      .catch((e) => {
        alert(e);
      });
  },

  data() {
    return {
      code: "",
      last_name: "",
      first_name: "",
      last_name_kana: "",
      first_name_kana: "",
      zipcode1: "",
      zipcode2: "",
      prefecture: "",
      city: "",
      building: "",
      tel1: "",
      tel2: "",
      tel3: "",
      mail: "",
      codeFlg: false,
      nameFlg: false,
      namekanaFlg: false,
      zipcodeFlg: false,
      prefectureFlg: false,
      cityFlg: false,
      telFlg: false,
      mailFlg: false,
      errors: {},
      selectedStaff: null,
      clearFlg: false,
      // items: [
      //     { code: 100, last_name: "aaa" },
      //     { code: 200, last_name: "bbb" },
      // ],
      items: [],
    };
  },
  methods: {
    post() {
      if(confirm("本当に新規追加しますか？")){
      var now = new Date();
      this.axios
        .post(
          "/api/StaffRegister/",
          {
            StaffID: this.code,
            StaffLastName: this.last_name,
            StaffFirstName: this.first_name,
            StaffLastNameKana: this.last_name_kana,
            StaffFirstNameKana: this.first_name_kana,
            ZipCode: this.zipcode1 + this.zipcode2,
            Prefecture: this.prefecture,
            City: this.city,
            Building: this.building,
            Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
            Mail: this.mail,
            DeleteDate: null,
            LastUpdate: now,
            LastAdd: now,
          },
          {
            headers: { "Content-Type": "application/x-www-form-urlencoded" },
          }
        )
        .then(() => {
          this.axios
            .get("/api/StaffListFunction/")
            .then((res) => {
              console.log(res.data);
              var string1 = JSON.stringify(res.data);
              let arr = JSON.parse(string1);
              console.log(arr);
              this.items = arr;
              this.items.sort((a, b) => {
                return a.staffID - b.staffID;
              });
              this.clear();
            })
            .catch((e) => {
              alert(e);
            });
        })
        .catch((e) => {
          alert(e);
        });
      }
    },
    put() {
      if(confirm("本当に更新しますか？")){
      var now = new Date();
      this.axios
        .post(
          "/api/StaffUpdate/",
          {
            ID: this.selectedStaff.id,
            StaffID: this.code,
            StaffLastName: this.last_name,
            StaffFirstName: this.first_name,
            StaffLastNameKana: this.last_name_kana,
            StaffFirstNameKana: this.first_name_kana,
            ZipCode: this.zipcode1 + this.zipcode2,
            Prefecture: this.prefecture,
            City: this.city,
            Building: this.building,
            Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
            Mail: this.mail,
            DeleteDate: null,
            LastUpdate: now,
            LastAdd: null,
          },
          {
            headers: { "Content-Type": "application/x-www-form-urlencoded" },
          }
        )
        .then(() => {
          this.axios
            .get("/api/StaffListFunction/")
            .then((res) => {
              console.log(res.data);
              var string1 = JSON.stringify(res.data);
              let arr = JSON.parse(string1);
              console.log(arr);
              this.items = arr;
              this.items.sort((a, b) => {
                return a.staffID - b.staffID;
              });
              this.clear();
            })
            .catch((e) => {
              alert(e);
            });
        })
        .catch((e) => {
          alert(e);
        });
      }
    },
    del() {
      if(confirm("本当に削除しますか？")){
      var now = new Date();
      this.axios
        .post(
          "/api/StaffDelete/",
          {
            ID: this.selectedStaff.id,
            StaffID: this.code,
            StaffLastName: this.last_name,
            StaffFirstName: this.first_name,
            StaffLastNameKana: this.last_name_kana,
            StaffFirstNameKana: this.first_name_kana,
            ZipCode: this.zipcode1 + this.zipcode2,
            Prefecture: this.prefecture,
            City: this.city,
            Building: this.building,
            Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
            Mail: this.mail,
            DeleteDate: now,
            LastUpdate: now,
            LastAdd: now,
          },
          {
            headers: { "Content-Type": "application/x-www-form-urlencoded" },
          }
        )
        .then(() => {
          this.axios
            .get("/api/StaffListFunction/")
            .then((res) => {
              console.log(res.data);
              var string1 = JSON.stringify(res.data);
              let arr = JSON.parse(string1);
              console.log(arr);
              this.items = arr;
              this.items.sort((a, b) => {
                return a.staffID - b.staffID;
              });
              this.clear();
            })
            .catch((e) => {
              alert(e);
            });
        })
        .catch((e) => {
          alert(e);
        });
      }
    },
    clear() {
        this.code = "";
        this.last_name = "";
        this.first_name = "";
        this.last_name_kana = "";
        this.first_name_kana = "";
        this.zipcode1 = "";
        this.zipcode2 = "";
        this.prefecture = "";
        this.city = "";
        this.building = "";
        this.tel1 = "";
        this.tel2 = "";
        this.tel3 = "";
        this.mail = "";
        this.codeFlg = false;
        this.nameFlg = false;
        this.namekanaFlg = false;
        this.zipcodeFlg = false;
        this.prefectureFlg = false;
        this.cityFlg = false;
        this.telFlg = false;
        this.mailFlg = false;
        this.clearFlg = true;
        this.selectedStaff = null;
        this.disabledFlg = false;
        this.errors = {};
        this.$nextTick(() =>{
          this.clearFlg = false;
        })
      // location.reload();
    },
    selectRow(item) {
      this.selectedStaff = item;
      this.code = String(item.staffID);
      this.last_name = item.staffLastName;
      this.first_name = item.staffFirstName;
      this.last_name_kana = item.staffLastNameKana;
      this.first_name_kana = item.staffFirstNameKana;
      this.zipcode1 = String(item.zipCode).slice(0, 3);
      this.zipcode2 = String(item.zipCode).slice(3, 7);
      this.prefecture = item.prefecture;
      this.city = item.city;
      this.building = item.building;
      var tel = item.tel.split("-");
      this.tel1 = tel[0];
      this.tel2 = tel[1];
      this.tel3 = tel[2];
      this.mail = item.mail;
    },
  },
  computed: {
    // 必須入力項目が未入力の場合、送信ボタンが非活性化する
    isDisabled() {
      return this.codeFlg == true &&
        this.nameFlg == true &&
        this.namekanaFlg == true &&
        this.zipcodeFlg == true &&
        this.prefectureFlg == true &&
        this.cityFlg == true &&
        this.telFlg == true &&
        this.mailFlg == true &&
        this.selectedStaff == null &&
        Object.keys(this.errors).length == 0
        ? false
        : true;
    },
    editDisabled() {
      return this.codeFlg == true &&
        this.nameFlg == true &&
        this.namekanaFlg == true &&
        this.zipcodeFlg == true &&
        this.prefectureFlg == true &&
        this.cityFlg == true &&
        this.telFlg == true &&
        this.mailFlg == true &&
        this.selectedStaff != null &&
        Object.keys(this.errors).length == 0
        ? false
        : true;
    },

  },
  watch: {
    // リアルタイムでバリデーションチェックを行う
    code(code) {
      if (this.clearFlg == false) {
        if (!code) {
          this.$set(this.errors, "code", "必須入力項目です。");
          this.codeFlg = false;
        } else if (code.match(/^([1-9]\d*|0)$/)) {
          this.$delete(this.errors, "code");
          this.codeFlg = true;
        } else {
          this.$set(this.errors, "code", "半角数字で入力してください。");
          this.codeFlg = false;
        }
      } 
    },
    last_name(last_name) {
      if (this.clearFlg == false) {
        if (!last_name || !this.first_name) {
          this.$set(this.errors, "name", "必須入力項目です。");
          this.nameFlg = false;
        } else if (last_name.length > 50) {
          this.$set(this.errors, "name", "50文字以内で入力してください。");
          this.nameFlg = false;
        } else {
          this.$delete(this.errors, "name");
          this.nameFlg = true;
        }
      } 
    },
    first_name(first_name) {
      if (this.clearFlg == false) {
        if (!first_name || !this.last_name) {
          this.$set(this.errors, "name", "必須入力項目です。");
          this.nameFlg = false;
        } else if (first_name.length > 50) {
          this.$set(this.errors, "name", "50文字以内で入力してください。");
          this.nameFlg = false;
        } else {
          this.$delete(this.errors, "name");
          this.nameFlg = true;
        }
      } 
    },
    last_name_kana(last_name_kana) {
      if (this.clearFlg == false) {
        if (!last_name_kana || !this.first_name_kana) {
          this.$set(this.errors, "namekana", "必須入力項目です。");
          this.namekanaFlg = false;
        } else if (
          !last_name_kana.match(/^[\u30a0-\u30ff]+$/) &&
          !this.first_name_kana.match(/^[\u30a0-\u30ff]+$/)
        ) {
          this.$set(this.errors, "namekana", "カタカナで入力してください。");
          this.namekanaFlg = false;
        } else if (last_name_kana.length > 50) {
          this.$set(this.errors, "namekana", "50文字以内で入力してください。");
          this.namekanaFlg = false;
        } else {
          this.$delete(this.errors, "namekana");
          this.namekanaFlg = true;
        }
      } 
    },
    first_name_kana(first_name_kana) {
      if (this.clearFlg == false) {
        if (!first_name_kana || !this.last_name_kana) {
          this.$set(this.errors, "namekana", "必須入力項目です。");
          this.namekanaFlg = false;
        } else if (
          !first_name_kana.match(/^[\u30a0-\u30ff]+$/) &&
          !this.last_name_kana.match(/^[\u30a0-\u30ff]+$/)
        ) {
          this.$set(this.errors, "namekana", "カタカナで入力してください。");
          this.namekanaFlg = false;
        } else if (first_name_kana.length > 50) {
          this.$set(this.errors, "namekana", "50文字以内で入力してください。");
          this.namekanaFlg = false;
        } else {
          this.$delete(this.errors, "namekana");
          this.namekanaFlg = true;
        }
      } 
    },
    zipcode1(zipcode1) {
      if (this.clearFlg == false) {
        if (!zipcode1 || !this.zipcode2) {
          this.$set(this.errors, "zipcode", "必須入力項目です。");
          this.zipcodeFlg = false;
        } else if (
          zipcode1.match(/^[1-9]{1}[0-9]{2}$/) &&
          this.zipcode2.match(/^\d{4}$/)
        ) {
          this.$delete(this.errors, "zipcode");
          this.zipcodeFlg = true;
        } else {
          this.$set(
            this.errors,
            "zipcode",
            "先頭が0以外の半角数字で入力してください。（3桁＋4桁）"
          );
          this.zipcodeFlg = false;
        }
      } 
    },
    zipcode2(zipcode2) {
      if (this.clearFlg == false) {
        if (!this.zipcode1 || !zipcode2) {
          this.$set(this.errors, "zipcode", "必須入力項目です。");
          this.zipcodeFlg = false;
        } else if (
          this.zipcode1.match(/^[1-9]{1}[0-9]{2}$/) &&
          zipcode2.match(/^\d{4}$/)
        ) {
          this.$delete(this.errors, "zipcode");
          this.zipcodeFlg = true;
        } else {
          this.$set(
            this.errors,
            "zipcode",
            "先頭が0以外の半角数字で入力してください。（3桁＋4桁）"
          );
          this.zipcodeFlg = false;
        }
      } 
    },
    prefecture(prefecture) {
      if (this.clearFlg == false) {
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
      } 
    },
    city(city) {
      if (this.clearFlg == false) {
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
      } 
    },
    building(building) {
      if (this.clearFlg == false) {
        if (building.length > 50) {
          this.$set(this.errors, "building", "50文字以内で入力してください。");
        } else {
          this.$delete(this.errors, "building");
        }
      } 
    },
    tel1(tel1) {
      if (this.clearFlg == false) {
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
      } 
    },
    tel2(tel2) {
      if (this.clearFlg == false) {
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
      } 
    },
    tel3(tel3) {
      if (this.clearFlg == false) {
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
      } 
    },
    mail(mail) {
      if (this.clearFlg == false) {
 
        if (!mail) {
          this.$set(this.errors, "mail", "必須入力項目です。");
          this.mailFlg = false;
        } else if (
          !mail.match(
            /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/
          )
        ) {
          this.$set(
            this.errors,
            "mail",
            "正しいメール形式で入力してください。"
          );
          this.mailFlg = false;
        } else if (mail.length > 40) {
          this.$set(this.errors, "mail", "40文字以内で入力してください。");
          this.mailFlg = false;
        } else {
          this.$delete(this.errors, "mail");
          this.mailFlg = true;
        }
      } 
    },
  },
};
</script>



<style scoped>
body {
  margin: 0;
}
.container {
  width: 100%;
  margin: 50px auto;
  text-align: center;
}
.item {
  display: inline-block;
  vertical-align: top;
  margin: auto 50px;
}

.datatable {
  overflow-y: scroll;
}
.data th {
  border: solid 1px;
  padding: 10px;
}
.data td {
  padding: 10px;
  border-right: solid 1px;
}
.highlight {
  background: rgb(230, 230, 230);
}
tr:hover {
  cursor: pointer;
}
table.data {
  border: solid 1px;
  border-collapse: collapse;
  width: 500px;
  height: 50px;
}
.row1 {
  width: 30%;
}
.row2 {
  width: 70%;
}
.fixed {
  position: sticky;
  top: 0;
  color: #fff;
  background: #333;
}
.fixed::before {
  content: "";
  position: absolute;
  top: -1px;
  left: -1px;
  width: 100%;
  height: 100%;
  border: 1px solid #ccc;
}
/* 
table {
  margin-left: auto;
  margin-right: auto;
} */
label {
  display: inline-block;
  white-space: nowrap;
}
.inputdev {
  width: 150px;
}
input {
  width: 300px;
  font-size: 20px;
  padding: 0px;
  margin-bottom: 15px;
}
button {
  width: 100px;
  height: 40px;
  margin: auto 15px;
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
.form th {
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