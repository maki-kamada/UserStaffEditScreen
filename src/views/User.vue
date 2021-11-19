<template>
<div class="container">
    <!-- <div id="app"> -->
    <form>
        <!-- 会社名フォーム -->
        <div class="container">
            <div>
                <label for="name">名前</label>
                <input type="text" v-model="name">
                <span>{{ errors.name }}</span>
            </div>
        </div>
    </form>
    <!-- </div> -->
    <div class="register">
        <button type="button" :disabled="isDisabled">送信</button>
    </div>
</div>
</template>



<script>

export default {
    data() {
        return {
            name: '',
            errors: {}
        }
    },
    computed: {
        isDisabled() {
            return Object.keys(this.errors).length > 0 ? true : false
        }
    },
    watch: {
        name(name) {
            if (name) {
                this.$delete(this.errors, 'name')
            } else {
                this.$set(this.errors, "name", '必須入力項目です。')
            }
        }
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