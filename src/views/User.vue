<template>
<div class="container">
    <form>
        <div class="container">
            <!-- 会社名フォーム -->
            <div>
                <label for="company">会社名</label>
                <input type="text" v-model="company">
                <span>{{ errors.company }}</span>
            </div>
            <!-- 代表者名フォーム -->
            <div>
                <label for="president">代表者名</label>
                <input type="text" v-model="president">
                <span>{{ errors.president }}</span>
            </div>
            <!-- 郵便番号フォーム -->
            <div>
                <label for="zipcode">郵便番号</label>
                <input type="text" v-model="zipcode1">
                <input type="text" v-model="zipcode2">
                <span>{{ errors.zipcode }}</span>
            </div>
            <!-- 住所１フォーム -->
            <div>
                <label for="prefecture">住所１(都道府県)</label>
                <input type="text" v-model="prefecture">
                <span>{{ errors.prefecture }}</span>
            </div>
            <!-- 住所２フォーム -->
            <div>
                <label for="city">住所２(市区町村)</label>
                <input type="text" v-model="city">
                <span>{{ errors.city }}</span>
            </div>
            <!-- 住所３フォーム -->
            <div>
                <label for="building">住所３(建物名等)</label>
                <input type="text" v-model="building">
                <span>{{ errors.building }}</span>
            </div>
            <!-- 電話番号フォーム -->
            <div>
                <label for="tel">電話番号</label>
                <input type="text" v-model="tel1">
                <input type="text" v-model="tel2">
                <input type="text" v-model="tel3">
                <span>{{ errors.tel }}</span>
            </div>
            <!-- FAXフォーム -->
            <div>
                <label for="fax">FAX</label>
                <input type="text" v-model="fax1">
                <input type="text" v-model="fax2">
                <input type="text" v-model="fax3">
                <span>{{ errors.fax }}</span>
            </div>
            <!-- e-mailフォーム -->
            <div>
                <label for="mail">e-mail</label>
                <input type="text" v-model="mail">
                <span>{{ errors.mail }}</span>
            </div>
        </div>
    </form>
    <div class="register">
        <button type="button" :disabled=isDisabled>送信</button>
    </div>
</div>
</template>



<script>

export default {
    data() {
        return {
            company: '',
            president: '',
            zipcode1: '',
            zipcode2: '',
            prefecture: '',
            city: '',
            building: '',
            tel1: '',
            tel2: '',
            tel3: '',
            fax1: '',
            fax2: '',
            fax3: '',
            mail: '',
            companyFlg: false,
            zipcodeFlg: false,
            prefectureFlg: false,
            cityFlg: false,
            telFlg: false,
            mailFlg: false,
            errors: {}
        }
    },
    computed: {     // 必須入力項目が未入力の場合、送信ボタンが非活性化する
        isDisabled() {
            return this.companyFlg == true && this.zipcodeFlg == true && this.prefectureFlg == true && this.cityFlg == true && this.telFlg == true && this.mailFlg == true && Object.keys(this.errors).length == 0 ? false : true
        }
    },
    watch: {        // リアルタイムでバリデーションチェックを行う
        company(company) {
            if (!company) {
                this.$set(this.errors, "company", '必須入力項目です。')
                this.companyFlg = false;
            } else if (company.length > 50) {
                this.$set(this.errors, "company", '50文字以内で入力してください。')
                this.companyFlg = false;
            } else {
                this.$delete(this.errors, 'company')
                this.companyFlg = true;
            }
        },
        president(president) {
            if (president.length > 50) {
                this.$set(this.errors, "president", '50文字以内で入力してください。')
            } else {
                this.$delete(this.errors, 'president')
            }
        },
        zipcode1(zipcode1) {
            if (!zipcode1 || !this.zipcode2) {
                this.$set(this.errors, "zipcode", '必須入力項目です。')
                this.zipcodeFlg = false;
            } else if (zipcode1.match(/^\d{3}$/) && this.zipcode2.match(/^\d{4}$/)) {
                this.$delete(this.errors, 'zipcode')
                this.zipcodeFlg = true;
            } else {
                this.$set(this.errors, "zipcode", '半角数字で入力してください。（3桁＋4桁）')
                this.zipcodeFlg = false;
            }
        },
        zipcode2(zipcode2) {
            if (!this.zipcode1 || !zipcode2) {
                this.$set(this.errors, "zipcode", '必須入力項目です。')
                this.zipcodeFlg = false;
            } else if (this.zipcode1.match(/^\d{3}$/) && zipcode2.match(/^\d{4}$/)) {
                this.$delete(this.errors, 'zipcode')
                this.zipcodeFlg = true;
            } else {
                this.$set(this.errors, "zipcode", '半角数字で入力してください。（3桁＋4桁）')
                this.zipcodeFlg = false;
            }
        },
        prefecture(prefecture) {
            if (!prefecture) {
                this.$set(this.errors, "prefecture", '必須入力項目です。')
                this.prefectureFlg = false;
            } else if (prefecture.length > 50) {
                this.$set(this.errors, "president", '50文字以内で入力してください。')
                this.prefectureFlg = false;
            } else {
                this.$delete(this.errors, 'prefecture')
                this.prefectureFlg = true;
            }
        },
        city(city) {
            if (!city) {
                this.$set(this.errors, "city", '必須入力項目です。')
                this.cityFlg = false;
            } else if (city.length > 50) {
                this.$set(this.errors, "city", '50文字以内で入力してください。')
                this.cityFlg = false;
            } else {
                this.$delete(this.errors, 'city')
                this.cityFlg = true;
            }
        },
        building(building) {
            if (building.length > 50) {
                this.$set(this.errors, "building", '50文字以内で入力してください。')
            } else {
                this.$delete(this.errors, 'building')
            }
        },
        tel1(tel1) {
            if (!tel1 || !this.tel2 || !this.tel3) {
                this.$set(this.errors, "tel", '必須入力項目です。')
                this.telFlg = false;
            } else if (tel1.match(/^\d{2,3}$/) && this.tel2.match(/^\d{1,4}$/) && this.tel3.match(/^\d{4}$/)) {
                this.$delete(this.errors, 'tel')
                this.telFlg = true;
            } else {
                this.$set(this.errors, "tel", '半角数字で入力してください。（2~3桁＋1~4桁＋4桁）')
                this.telFlg = false;
            }
        },
        tel2(tel2) {
            if (!this.tel1 || !tel2 || !this.tel3) {
                this.$set(this.errors, "tel", '必須入力項目です。')
                this.telFlg = false;
            } else if (this.tel1.match(/^\d{2,3}$/) && tel2.match(/^\d{1,4}$/) && this.tel3.match(/^\d{4}$/)) {
                this.$delete(this.errors, 'tel')
                this.telFlg = true;
            } else {
                this.$set(this.errors, "tel", '半角数字で入力してください。（2~3桁＋1~4桁＋4桁）')
                this.telFlg = false;
            }
        },
        tel3(tel3) {
            if (!this.tel1 || !this.tel2 || !tel3) {
                this.$set(this.errors, "tel", '必須入力項目です。')
                this.telFlg = false;
            } else if (this.tel1.match(/^\d{2,3}$/) && this.tel2.match(/^\d{1,4}$/) && tel3.match(/^\d{4}$/)) {
                this.$delete(this.errors, 'tel')
                this.telFlg = true;
            } else {
                this.$set(this.errors, "tel", '半角数字で入力してください。（2~3桁＋1~4桁＋4桁）')
                this.telFlg = false;
            }
        },
        fax1(fax1) {
            if (fax1 == "" && this.fax2 == "" && this.fax3 == "") {
                this.$delete(this.errors, 'fax')
            } else if (!fax1.match(/^\d{2,3}$/) || !this.fax2.match(/^\d{1,4}$/) || !this.fax3.match(/^\d{4}$/)) {
                this.$set(this.errors, "fax", '半角数字で入力してください。（2~3桁＋1~4桁＋4桁）')
            } else {
                this.$delete(this.errors, 'fax')
            }
        },
        fax2(fax2) {
            if (this.fax1 == "" && fax2 == "" && this.fax3 == "") {
                this.$delete(this.errors, 'fax')
            } else if (!this.fax1.match(/^\d{2,3}$/) || fax2.match(/^\d{1,4}$/) || !this.fax3.match(/^\d{4}$/)) {
                this.$set(this.errors, "fax", '半角数字で入力してください。（2~3桁＋1~4桁＋4桁）')
            } else {
                this.$delete(this.errors, 'fax')
            }
        },
        fax3(fax3) {
            if (this.fax1 == "" && this.fax2 == "" && fax3 == "") {
                this.$delete(this.errors, 'fax')
            } else if (!this.fax1.match(/^\d{2,3}$/) || !this.fax2.match(/^\d{1,4}$/) || !fax3.match(/^\d{4}$/)) {
                this.$set(this.errors, "fax", '半角数字で入力してください。（2~3桁＋1~4桁＋4桁）')
            } else {
                this.$delete(this.errors, 'fax')
            }
        },
        mail(mail) {
            if (!mail) {
                this.$set(this.errors, "mail", '必須入力項目です。')
                this.mailFlg = false;
            } else if (!mail.match(/^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/)) {
                this.$set(this.errors, "mail", '正しいメール形式で入力してください。')
                this.mailFlg = false;
            } else if (mail.length > 40) {
                this.$set(this.errors, "mail", '40文字以内で入力してください。')
                this.mailFlg = false;
            } else {
                this.$delete(this.errors, 'mail')
                this.mailFlg = true;
            }
        },
    }
};
</script>



<style scoped>
.container {
    width: 380px;
    margin: 50px auto;
  }
  label {
    display: block;
    margin: 15px 0px;
  }
  input {
    width: 100%;
    height: 30px;
    font-size: 20px;
  }
  button {
    width: 130px;
    height: 40px;
  }
  .register {
    width: 100%;
    text-align: center;
    margin: 50px 0;
  }
  span {
    height: 20px;
    font-size: 14px;
    color: red;
    width: 90%;
    display: block;
  }
</style>