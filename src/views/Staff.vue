<template>
  <div class="container">
    <p>社員マスタ</p>
    <div class="item datatable">
      <table class="data">
        <thead>
          <tr>
            <th class="column1 fixed">社員コード</th>
            <th class="column2 fixed">社員名</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="row in table"
            :key="row.staffID"
            @click="selectRow(row)"
            :class="{ highlight: row == selectedStaff }"
          >
            <td class="column1">{{ row.staffID }}</td>
            <td class="column2">
              {{ row.lastName }} {{ row.firstName }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="item">
      <form>
        <table class="form">
          <!-- 社員コードフォーム -->
          <tr>
            <div>
              <th><label for="id">社員コード</label></th>
              <td><input type="text" v-model="id" /></td>
              <td class="errMsg">
                <span>{{ errors.id }}</span>
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
        <button type="button" :disabled="isDisabled" @click="addStaff">
          新規追加
        </button>
        <button type="button" :disabled="editDisabled" @click="updateStaff">
          更新
        </button>
        <button type="button" :disabled="editDisabled" @click="deleteStaff">
          削除
        </button>
      </div>
    </div>
  </div>
</template>



<script>


export default {

  //テーブルを初期表示するためのget通信
  created() {
    this.getStaff();
  },

  data() {
    return {
      id: "",
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
      idFlg: false,
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
      table: [],
    };
  },
  methods: {

    //社員テーブルデータを取得。
    getStaff() {
      this.axios
        .get("/api/GetM_Staff/")
        .then((res) => {
          console.log(res.data);
          var string1 = JSON.stringify(res.data);
          let arr = JSON.parse(string1);
          console.log(arr);
          this.table = arr;
          this.table.sort((a, b) => {
            return a.staffID - b.staffID;
          });
          this.clear();
        })
        .catch((e) => {
          console.log(e);
        });
    },

    //テーブルへのデータ新規追加
    addStaff() {
      if (confirm("本当に新規追加しますか？")) {
        var pNow = new Date();
        this.axios
          .post(
            "/api/StaffAdd/",
            {
              StaffID: this.id,
              LastName: this.last_name,
              FirstName: this.first_name,
              LastNameKana: this.last_name_kana,
              FirstNameKana: this.first_name_kana,
              ZipCode: this.zipcode1 + this.zipcode2,
              Prefecture: this.prefecture,
              City: this.city,
              Building: this.building,
              Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
              Mail: this.mail,
              DeleteDate: null,
              LastUpdate: pNow,
              LastAdd: pNow,
            },
            {
              headers: { "Content-Type": "application/x-www-form-urlencoded" },
            }
          )
          .then(() => {
            this.getStaff();
          })
          .catch((e) => {
            alert(e);
          });
      }
    },

    //テーブル内、選択された行データを更新
    updateStaff() {
      if (confirm("本当に更新しますか？")) {
        var pNow = new Date();
        this.axios
          .post(
            "/api/StaffUpdate/",
            {
              ID: this.selectedStaff.id,
              StaffID: this.id,
              LastName: this.last_name,
              FirstName: this.first_name,
              LastNameKana: this.last_name_kana,
              FirstNameKana: this.first_name_kana,
              ZipCode: this.zipcode1 + this.zipcode2,
              Prefecture: this.prefecture,
              City: this.city,
              Building: this.building,
              Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
              Mail: this.mail,
              DeleteDate: null,
              LastUpdate: pNow,
              LastAdd: null,
            },
            {
              headers: { "Content-Type": "application/x-www-form-urlencoded" },
            }
          )
          .then(() => {
            this.getStaff();
          })
          .catch((e) => {
            alert(e);
          });
      }
    },

    //テーブル内、選択された行データを削除
    deleteStaff() {
      if (confirm("本当に削除しますか？")) {
        var pNow = new Date();
        this.axios
          .post(
            "/api/StaffDelete/",
            {
              ID: this.selectedStaff.id,
              StaffID: this.id,
              LastName: this.last_name,
              FirstName: this.first_name,
              LastNameKana: this.last_name_kana,
              FirstNameKana: this.first_name_kana,
              ZipCode: this.zipcode1 + this.zipcode2,
              Prefecture: this.prefecture,
              City: this.city,
              Building: this.building,
              Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
              Mail: this.mail,
              DeleteDate: pNow,
              LastUpdate: pNow,
              LastAdd: pNow,
            },
            {
              headers: { "Content-Type": "application/x-www-form-urlencoded" },
            }
          )
          .then(() => {
            this.getStaff();
          })
          .catch((e) => {
            alert(e);
          });
      }
    },

    //入力欄をクリア。テーブル行選択をクリア。
    clear() {
      this.id = "";
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
      this.idFlg = false;
      this.nameFlg = false;
      this.namekanaFlg = false;
      this.zipcodeFlg = false;
      this.prefectureFlg = false;
      this.cityFlg = false;
      this.telFlg = false;
      this.mailFlg = false;
      this.clearFlg = true;
      this.selectedStaff = null;
      this.errors = {};
      this.$nextTick(() => {
        this.clearFlg = false;
      });
    },

    //テーブル行選択時の、入力欄へのデータ表示
    selectRow(row) {
      this.selectedStaff = row;
      this.id = String(row.staffID);
      this.last_name = row.lastName;
      this.first_name = row.firstName;
      this.last_name_kana = row.lastNameKana;
      this.first_name_kana = row.firstNameKana;
      this.zipcode1 = String(row.zipCode).slice(0, 3);
      this.zipcode2 = String(row.zipCode).slice(3, 7);
      this.prefecture = row.prefecture;
      this.city = row.city;
      this.building = row.building;
      var tel = row.tel.split("-");
      this.tel1 = tel[0];
      this.tel2 = tel[1];
      this.tel3 = tel[2];
      this.mail = row.mail;
    },
  },

  computed: {
    //エラー文がなく、テーブル内に選択された行がないとき、新規追加ボタンが活性化する
    isDisabled() {
      return this.idFlg == true &&
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

    // エラー文がなく、テーブル内に選択された行があるとき、削除・更新ボタンが活性化する
    editDisabled() {
      return this.idFlg == true &&
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

  // リアルタイムでバリデーションチェックを行う
  watch: {
    id(id) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "id");
          this.idFlg = true;
          if (!id) {
            this.$set(this.errors, "id", "必須入力項目です。");
            this.idFlg = false;
            return
          }
          if (!id.match(/^([1-9]\d*|0)$/)) {
            this.$set(this.errors, "id", "半角数字で入力してください。");
            this.idFlg = false;
            return
          } 
      }
    },

    last_name(last_name) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "name");
          this.nameFlg = true;
          if (!last_name || !this.first_name) {
            this.$set(this.errors, "name", "必須入力項目です。");
            this.nameFlg = false;
            return
          }
          if (last_name.length > 50) {
            this.$set(this.errors, "name", "50文字以内で入力してください。");
            this.nameFlg = false;
            return
          } 
      }
    },

    first_name(first_name) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "name");
          this.nameFlg = true;
          if (!first_name || !this.last_name) {
            this.$set(this.errors, "name", "必須入力項目です。");
            this.nameFlg = false;
            return
          }
          if (first_name.length > 50) {
            this.$set(this.errors, "name", "50文字以内で入力してください。");
            this.nameFlg = false;
            return
          } 
      }
    },

    last_name_kana(last_name_kana) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "namekana");
          this.namekanaFlg = true
          if (!last_name_kana || !this.first_name_kana) {
            this.$set(this.errors, "namekana", "必須入力項目です。");
            this.namekanaFlg = false;
            return 
            }
          if (
            !last_name_kana.match(/^[\u30a0-\u30ff]+$/) ||
            !this.first_name_kana.match(/^[\u30a0-\u30ff]+$/)
          ) {
            this.$set(this.errors, "namekana", "カタカナで入力してください。");
            this.namekanaFlg = false;
            return
          }
          if (last_name_kana.length > 50) {
            this.$set(
              this.errors,
              "namekana",
              "50文字以内で入力してください。"
            );
            this.namekanaFlg = false;
            return
          } 
          // else {
          //   this.$delete(this.errors, "namekana");
            // this.namekanaFlg = true;
      }
    },

    first_name_kana(first_name_kana) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "namekana");
          this.namekanaFlg = true;
          if (!first_name_kana || !this.last_name_kana) {
            this.$set(this.errors, "namekana", "必須入力項目です。");
            this.namekanaFlg = false;
            return
          }
          if (
            !first_name_kana.match(/^[\u30a0-\u30ff]+$/) ||
            !this.last_name_kana.match(/^[\u30a0-\u30ff]+$/)
          ) {
            this.$set(this.errors, "namekana", "カタカナで入力してください。");
            this.namekanaFlg = false;
            return
          }
          if (first_name_kana.length > 50) {
            this.$set(
              this.errors,
              "namekana",
              "50文字以内で入力してください。"
            );
            this.namekanaFlg = false;
            return
          } 
      }
    },

    zipcode1(zipcode1) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "zipcode");
          this.axios
              .get(
                "https://zipcloud.ibsnet.co.jp/api/search?zipcode=" +
                  this.zipcode1 +
                  this.zipcode2
              )
              .then((res) => {
                this.prefecture = res.data.results[0].address1;
                this.city =
                  res.data.results[0].address2 + res.data.results[0].address3;
              });
          this.zipcodeFlg = true;
          if (!zipcode1 || !this.zipcode2) {
            this.$set(this.errors, "zipcode", "必須入力項目です。");
            this.zipcodeFlg = false;
            return
          }
          if (
            !zipcode1.match(/^[1-9]{1}[0-9]{2}$/) ||
            !this.zipcode2.match(/^\d{4}$/)
          ) {
            this.$set(
              this.errors,
              "zipcode",
              "先頭が0以外の半角数字で入力してください。（3桁＋4桁）"
            );
            this.zipcodeFlg = false;
            return
          } 
      }
    },

    //郵便番号から住所を自動取得
    zipcode2(zipcode2) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "zipcode");
            this.zipcodeFlg = true;
            this.axios
              .get(
                "https://zipcloud.ibsnet.co.jp/api/search?zipcode=" +
                  this.zipcode1 +
                  this.zipcode2
              )
              .then((res) => {
                this.prefecture = res.data.results[0].address1;
                this.city =
                  res.data.results[0].address2 + res.data.results[0].address3;
              });
          if (!this.zipcode1 || !zipcode2) {
            this.$set(this.errors, "zipcode", "必須入力項目です。");
            this.zipcodeFlg = false;
            return
          }
          if (
            !this.zipcode1.match(/^[1-9]{1}[0-9]{2}$/) ||
            !zipcode2.match(/^\d{4}$/)
          ) {
            this.$set(
              this.errors,
              "zipcode",
              "先頭が0以外の半角数字で入力してください。（3桁＋4桁）"
            );
            this.zipcodeFlg = false;
            return
          } 
      }
    },

    prefecture(prefecture) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "prefecture");
          this.prefectureFlg = true;
          if (!prefecture) {
            this.$set(this.errors, "prefecture", "必須入力項目です。");
            this.prefectureFlg = false;
            return
          }
          if (prefecture.length > 50) {
            this.$set(
              this.errors,
              "prefecture",
              "50文字以内で入力してください。"
            );
            this.prefectureFlg = false;
            return
          }
      }
    },

    city(city) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "city");
          this.cityFlg = true;
          if (!city) {
            this.$set(this.errors, "city", "必須入力項目です。");
            this.cityFlg = false;
            return
          }
          if (city.length > 50) {
            this.$set(this.errors, "city", "50文字以内で入力してください。");
            this.cityFlg = false;
            return
          } 
      }
    },

    building(building) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "building");
          if (building.length > 50) {
            this.$set(
              this.errors,
              "building",
              "50文字以内で入力してください。"
            );
            return
          }
      }
    },

    tel1(tel1) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "tel");
          this.telFlg = true;
          if (!tel1 || !this.tel2 || !this.tel3) {
            this.$set(this.errors, "tel", "必須入力項目です。");
            this.telFlg = false;
            return
          }
          if (
            !tel1.match(/^\d{2,3}$/) ||
            !this.tel2.match(/^\d{1,4}$/) ||
            !this.tel3.match(/^\d{4}$/)
          ) {
            this.$set(
              this.errors,
              "tel",
              "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
            );
            this.telFlg = false;
            return
          } 
      }
    },

    tel2(tel2) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "tel");
          this.telFlg = true;
          if (!this.tel1 || !tel2 || !this.tel3) {
            this.$set(this.errors, "tel", "必須入力項目です。");
            this.telFlg = false;
            return
          }
          if (
            !this.tel1.match(/^\d{2,3}$/) ||
            !tel2.match(/^\d{1,4}$/) ||
            !this.tel3.match(/^\d{4}$/)
          ) {
            this.$set(
              this.errors,
              "tel",
              "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
            );
            this.telFlg = false;
            return
          } 
      }
    },
    
    tel3(tel3) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "tel");
          this.telFlg = true;
          if (!this.tel1 || !this.tel2 || !tel3) {
            this.$set(this.errors, "tel", "必須入力項目です。");
            this.telFlg = false;
            return
          }
          if (
            !this.tel1.match(/^\d{2,3}$/) ||
            !this.tel2.match(/^\d{1,4}$/) ||
            !tel3.match(/^\d{4}$/)
          ) {
            this.$set(
              this.errors,
              "tel",
              "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
            );
            this.telFlg = false;
            return
          } 
      }
    },

    mail(mail) {
      if (this.clearFlg == false) {
          this.$delete(this.errors, "mail");
          this.mailFlg = true;
          if (!mail) {
            this.$set(this.errors, "mail", "必須入力項目です。");
            this.mailFlg = false;
            return
          }
          if (
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
            return
          }
          if (mail.length > 40) {
            this.$set(this.errors, "mail", "40文字以内で入力してください。");
            this.mailFlg = false;
            return
          } 
      }
    },

  },
};
</script>


<style scoped>
@import "../css/style.css";
</style>