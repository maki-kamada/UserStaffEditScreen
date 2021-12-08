<template>
  <div class="container">
    <p>顧客マスタ</p>
    <div class="item datatable">
      <table class="data">
        <thead>
          <tr>
            <th class="column1 fixed">ID</th>
            <th class="column2 fixed">会社名</th>
          </tr>
        </thead>
        <tbody class="test">
          <tr
            v-for="row in table"
            :key="row.userID"
            @click="selectRow(row)"
            :class="{ highlight: row == selectedUser }"
          >
            <td class="column1">{{ row.userID }}</td>
            <td class="column2">{{ row.companyName }}</td>
          </tr>
        </tbody>
      </table>

    </div>
    <div class="item">
      <form>
        <table class="form">
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
        </table>
      </form>
      <div class="register">
        <button type="button" @click="clear">クリア</button>
        <button type="button" :disabled="isDisabled" @click="addUser">
          新規追加
        </button>
        <button type="button" :disabled="editDisabled" @click="updateUser">更新</button>
        <button type="button" :disabled="editDisabled" @click="deleteUser">削除</button>
      </div>
    </div>
  </div>
</template>



<script>


export default {
  //テーブルを初期表示するためのget通信
  created() {
    this.getUser()

  },

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
      selectedUser: null,
      clearFlg: false,
      table: [],
    };
  },
  methods: {
    //顧客テーブルデータを取得。
    getUser(){
      this.axios
            .get("/api/GetM_User/")
            .then((res) => {
              console.log(res.data);
              var string1 = JSON.stringify(res.data);
              let arr = JSON.parse(string1);
              console.log(arr);
              this.table = arr;
              this.clear();
            })
            .catch((e) => {
              console.log(e);
            });

    },

    //テーブルへのデータ新規追加
    addUser() {
      if (confirm("本当に新規追加しますか？")) {
      var pNow = new Date();
      var pFax = "";
      if(this.fax1 != ""){
        pFax = this.fax1 + "-" + this.fax2 + "-" + this.fax3;
      }else{ 
        pFax = "";
      }
      this.axios
        .post(
          "/api/UserAdd/",
          {
            CompanyName: this.company,
            PresidentName: this.president,
            ZipCode: this.zipcode1 + this.zipcode2,
            Prefecture: this.prefecture,
            City: this.city,
            Building: this.building,
            Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
            Fax: pFax,
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
          this.getUser()
        })
        .catch((e) => {
          alert(e);
        });
      }
    },

    //テーブル内、選択された行データを更新
    updateUser() {
      if (confirm("本当に更新しますか？")) {
      var pNow = new Date();
      var pFax;
      if(this.fax1 != ""){
        pFax = this.fax1 + "-" + this.fax2 + "-" + this.fax3;
      }else{ 
        pFax = "";
      }
      this.axios
        .post(
          "/api/UserUpdate/",
          {
            UserID: this.selectedUser.userID,
            CompanyName: this.company,
            PresidentName: this.president,
            ZipCode: this.zipcode1 + this.zipcode2,
            Prefecture: this.prefecture,
            City: this.city,
            Building: this.building,
            Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
            Fax: pFax,
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
          this.getUser()
        })
        .catch((e) => {
          alert(e);
        });
      }
    },

    //テーブル内、選択された行データを削除
    deleteUser() {
      if (confirm("本当に削除しますか？")) {
      var pNow = new Date();
      var pFax;
      if(this.fax1 != ""){
        pFax = this.fax1 + "-" + this.fax2 + "-" + this.fax3;
      }else{ 
        pFax = "";
      }
      this.axios
        .post(
          "/api/UserDelete/",
          {
            UserID: this.selectedUser.userID,
            CompanyName: this.company,
            PresidentName: this.president,
            ZipCode: this.zipcode1 + this.zipcode2,
            Prefecture: this.prefecture,
            City: this.city,
            Building: this.building,
            Tel: this.tel1 + "-" + this.tel2 + "-" + this.tel3,
            Fax: pFax,
            Mail: this.mail,
            DeleteDate: pNow,
            LastUpdate: pNow,
            LastAdd: null,
          },
          {
            headers: { "Content-Type": "application/x-www-form-urlencoded" },
          }
        )
        .then(() => {
          this.getUser()
        })
        .catch((e) => {
          alert(e);
        });
      }
    },

    //入力欄をクリア。テーブル行選択をクリア。
    clear() {
      this.company = "";
      this.president = "";
      this.zipcode1 = "";
      this.zipcode2 = "";
      this.prefecture = "";
      this.city = "";
      this.building = "";
      this.tel1 = "";
      this.tel2 = "";
      this.tel3 = "";
      this.fax1 = "";
      this.fax2 = "";
      this.fax3 = "";
      this.mail = "";
      this.companyFlg = false;
      this.zipcodeFlg = false;
      this.prefectureFlg = false;
      this.cityFlg = false;
      this.telFlg = false;
      this.mailFlg = false;
      this.clearFlg = true;
      this.selectedUser = null;
      this.errors = {};
      this.$nextTick(() => {
        this.clearFlg = false;
      });
    },

    //テーブル行選択時の、入力欄へのデータ表示
    selectRow(row) {
      this.selectedUser = row;
      this.company = row.companyName;
      this.president = row.presidentName;
      this.zipcode1 = String(row.zipCode).slice(0, 3);
      this.zipcode2 = String(row.zipCode).slice(3, 7);
      this.prefecture = row.prefecture;
      this.city = row.city;
      this.building = row.building;
      var tel = row.tel.split("-");
      this.tel1 = tel[0];
      this.tel2 = tel[1];
      this.tel3 = tel[2];
      if (row.fax != "") {
        var fax = row.fax.split("-");
        this.fax1 = fax[0];
        this.fax2 = fax[1];
        this.fax3 = fax[2];
      }
      this.mail = row.mail;
    },
  },

  computed: {
    //エラー文がなく、テーブル内に選択された行がないとき、新規追加ボタンが活性化する
    isDisabled() {
      return this.companyFlg == true &&
        this.zipcodeFlg == true &&
        this.prefectureFlg == true &&
        this.cityFlg == true &&
        this.telFlg == true &&
        this.mailFlg == true &&
        this.selectedUser == null &&
        Object.keys(this.errors).length == 0
        ? false
        : true;
    },

    // エラー文がなく、テーブル内に選択された行があるとき、削除・更新ボタンが活性化する
    editDisabled() {
      return this.companyFlg == true &&
        this.zipcodeFlg == true &&
        this.prefectureFlg == true &&
        this.cityFlg == true &&
        this.telFlg == true &&
        this.mailFlg == true &&
        this.selectedUser != null &&
        Object.keys(this.errors).length == 0
        ? false
        : true;
    }
  },

  // リアルタイムでバリデーションチェックを行う
  watch: {
    company(company) {
      if (this.clearFlg == false) {
        this.$delete(this.errors, "company");
        this.companyFlg = true;
        if (!company) {
          this.$set(this.errors, "company", "必須入力項目です。");
          this.companyFlg = false;
          return
        }
        if (company.length > 50) {
          this.$set(this.errors, "company", "50文字以内で入力してください。");
          this.companyFlg = false;
          return
        }
      }
    },

    president(president) {
      if (this.clearFlg == false) {
        this.$delete(this.errors, "president");
        if (president.length > 50) {
          this.$set(this.errors, "president", "50文字以内で入力してください。");
        }
      }
    },

    zipcode1(zipcode1) {
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

    fax1(fax1) {
      if (this.clearFlg == false) {
        this.$delete(this.errors, "fax");
        if (fax1 == "" && this.fax2 == "" && this.fax3 == "") {
          this.$delete(this.errors, "fax");
          return
        }
        if (
          !fax1.match(/^\d{2,3}$/) ||
          !this.fax2.match(/^\d{1,4}$/) ||
          !this.fax3.match(/^\d{4}$/)
        ) {
          this.$set(
            this.errors,
            "fax",
            "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
          );
          return
        } 
      }
    },

    fax2(fax2) {
      if (this.clearFlg == false) {
        this.$delete(this.errors, "fax");
        if (this.fax1 == "" && fax2 == "" && this.fax3 == "") {
          this.$delete(this.errors, "fax");
          return
        }
        if (
          !this.fax1.match(/^\d{2,3}$/) ||
          fax2.match(/^\d{1,4}$/) ||
          !this.fax3.match(/^\d{4}$/)
        ) {
          this.$set(
            this.errors,
            "fax",
            "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
          );
          return
        }
      }
    },
    
    fax3(fax3) {
      if (this.clearFlg == false) {
        this.$delete(this.errors, "fax");
        if (this.fax1 == "" && this.fax2 == "" && fax3 == "") {
          this.$delete(this.errors, "fax");
          return
        }
        if (
          !this.fax1.match(/^\d{2,3}$/) ||
          !this.fax2.match(/^\d{1,4}$/) ||
          !fax3.match(/^\d{4}$/)
        ) {
          this.$set(
            this.errors,
            "fax",
            "半角数字で入力してください。（2~3桁＋1~4桁＋4桁）"
          );
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
@import "../css/style.css" 

</style>