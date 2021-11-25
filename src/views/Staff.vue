<template>
<div class="container">
    <form>
        <table>
            <div class="container">
                <!-- 社員コードフォーム -->
                <tr>
                    <div>
                        <th><label for="code">社員コード</label></th>
                        <td><input type="text" v-model="code"></td>
                        <td class="errMsg"><span>{{ errors.code }}</span></td>
                    </div>
                </tr>
                <!-- 社員名フォーム -->
                <tr>
                    <div>
                        <th><label for="name">社員名</label></th>
                        <td><input type="text" v-model="last_name"></td>
                        <td><input type="text" v-model="first_name"></td>
                        <td class="errMsg"><span>{{ errors.name }}</span></td>
                    </div>
                </tr>

                <!-- 社員名（カナ）フォーム -->
                <tr>
                    <div>
                        <th><label for="namekana">社員名（カナ）</label></th>
                        <td><input type="text" v-model="last_name_kana"></td>
                        <td><input type="text" v-model="first_name_kana"></td>
                        <td class="errMsg"><span>{{ errors.namekana }}</span></td>
                    </div>
                </tr>

                <!-- 郵便番号フォーム -->
                <tr>
                    <div>
                        <th><label for="zipcode">郵便番号</label></th>
                        <td><input class="numberInput" type="text" v-model="zipcode1"></td>
                        <td><span class="hyphen">―</span></td>
                        <td><input class="numberInput" type="text" v-model="zipcode2"></td>
                        <td class="errMsg"><span>{{ errors.zipcode }}</span></td>
                    </div>
                </tr>
                <!-- 住所１フォーム -->
                <tr>
                    <div>
                        <th><label for="prefecture">住所１(都道府県)</label></th>
                        <td><input type="text" v-model="prefecture"></td>
                        <td class="errMsg"><span>{{ errors.prefecture }}</span></td>
                    </div>
                </tr>
                <!-- 住所２フォーム -->
                <tr>
                    <div>
                        <th><label for="city">住所２(市区町村)</label></th>
                        <td><input type="text" v-model="city"></td>
                        <td class="errMsg"><span>{{ errors.city }}</span></td>
                    </div>
                </tr>
                <!-- 住所３フォーム -->
                <tr>
                    <div>
                        <th><label for="building">住所３(建物名等)</label></th>
                        <td><input type="text" v-model="building"></td>
                        <td class="errMsg"><span>{{ errors.building }}</span></td>
                    </div>
                </tr>
                <!-- 電話番号フォーム -->
                <tr>
                    <div class="aaa">
                        <th><label for="tel">電話番号</label></th>
                        <td><input class="numberInput" type="text" v-model="tel1"></td>
                        <td><span class="hyphen">―</span></td>
                        <td><input class="numberInput" type="text" v-model="tel2"></td>
                        <td><span class="hyphen">―</span></td>
                        <td><input class="numberInput" type="text" v-model="tel3"></td>
                        <td class="errMsg"><span>{{ errors.tel }}</span></td>
                    </div>
                </tr>

                <!-- e-mailフォーム -->
                <tr>
                    <div>
                        <th><label for="mail">e-mail</label></th>
                        <td><input type="text" v-model="mail"></td>
                        <td class="errMsg"><span>{{ errors.mail }}</span></td>
                    </div>
                </tr>
            </div>
        </table>
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
            codeFlg: false,
            nameFlg: false,
            namekanaFlg: false,
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
            return this.codeFlg == true && this.nameFlg == true && this.namekanaFlg == true && this.zipcodeFlg == true && this.prefectureFlg == true && this.cityFlg == true && this.telFlg == true && this.mailFlg == true && Object.keys(this.errors).length == 0 ? false : true
        }
    },
    watch: {        // リアルタイムでバリデーションチェックを行う
        code(code) {
          if (!code) {
              this.$set(this.errors, "code", '必須入力項目です。')
              this.codeFlg = false;
          }else if (code.match(/^([1-9]\d*|0)$/)) {
              this.$delete(this.errors, 'code')
              this.codeFlg = true;
          }else {
              this.$set(this.errors, "code", '半角数字で入力してください。')
              this.codeFlg = false;
          }
        },
        last_name(last_name) {
            if (!last_name || this.first_name) {
                this.$set(this.errors, "name", '必須入力項目です。')
                this.nameFlg = false;
            } else if (last_name.length > 50) {
                this.$set(this.errors, "name", '50文字以内で入力してください。')
                this.nameFlg = false;
            } else {
                this.$delete(this.errors, 'name')
                this.nameFlg = true;
            }
        },
        first_name(first_name) {
            if (!first_name || this.last_name) {
                this.$set(this.errors, "name", '必須入力項目です。')
                this.nameFlg = false;
            } else if (first_name.length > 50) {
                this.$set(this.errors, "name", '50文字以内で入力してください。')
                this.nameFlg = false;
            } else {
                this.$delete(this.errors, 'name')
                this.nameFlg = true;
            }
        },
        last_name_kana(last_name_kana) {
            if (!last_name_kana || !this.first_name_kana) {
                this.$set(this.errors, "namekana", '必須入力項目です。')
                this.namekanaFlg = false;
            } else if (!last_name_kana.match(/^[\u30a0-\u30ff]+$/) && !this.first_name_kana.match(/^[\u30a0-\u30ff]+$/)) {
                this.$set(this.errors, "namekana", 'カタカナで入力してください。')
                this.namekanaFlg = false;
            }else if (last_name_kana.length > 50){
                this.$set(this.errors, "namekana", '50文字以内で入力してください。')
                this.namekanaFlg = false;

            }else {
                this.$delete(this.errors, 'namekana')
                this.namekanaFlg = true;
            }
        },
        first_name_kana(first_name_kana) {
            if (!first_name_kana || !this.last_name_kana) {
                this.$set(this.errors, "namekana", '必須入力項目です。')
                this.namekanaFlg = false;
            } else if (!first_name_kana.match(/^[\u30a0-\u30ff]+$/) && !this.last_name_kana.match(/^[\u30a0-\u30ff]+$/)) {
                this.$set(this.errors, "namekana", 'カタカナで入力してください。')
                this.namekanaFlg = false;
            }else if (first_name_kana.length > 50){
                this.$set(this.errors, "namekana", '50文字以内で入力してください。')
                this.namekanaFlg = false;

            }else {
                this.$delete(this.errors, 'namekana')
                this.namekanaFlg = true;
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