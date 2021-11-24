<template>
  <div class='staff'>
    <input :type="type" :v-model="inputValue" :placeholder="placeholder" :name="name" />
    <span
      class="error-text"
      v-if="placeholder.match(/^[\u30a0-\u30ff]+$/)"
      v-show="kanaValidation()"
      >カタカナで入力してください。</span
    >
    <span class="error-text" v-if="requiredCheck()"
      >必須入力項目です。</span
    >
  </div>
</template>

<script>
// import Form from '../views/Staff.vue'
export default {
    name: 'FormInputText',
    props:{
        placeholder: String,
        name: String
    },
    data() {
        return{
            inputValue: '',
            requiredFlag: false,
            kanaFlag: false,
        }
    },
    methods: {
        requiredCheck: function () {
           this.inputValue.trim()
           return this.inputValue.length > 0 ? false :true
        },
        kanaValidation: function () {
            this.inputValue.trim()
            var kanaPattern = /^[\u30a0-\u30ff]+$/;
            if(this.inputValue.match(kanaPattern)){
                return false
            }else{
                return true
            }
        },

    }
//     methods: {
    
//       validationPresence: function () {
//         this.staff[this.column] = this.inputValue.trim();
//         this.$emit('update-status-from-child', this.satff);
//         return this.inputValue.trim();

//     },
//       validationKana: function () {
//         var kanaPattern = /^[\u30a0-\u30ff]+$/;
//         var result = kanaPattern.test(this.inputValue.trim());
//         this.staff[this.column] = result;
//         this.$emit('update-status-from-child', this.staff);
//         return result;
//     }
//   },
  
    


}

</script>

<style scoped>

</style>