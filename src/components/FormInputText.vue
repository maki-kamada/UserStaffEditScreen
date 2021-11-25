<template>
  <div>
    <!-- nameが型の種類、required、maxlength -->
    <input
     :value="value"
     @input="$emit('input', $event.target.value)"
     required
     :placeholder="placeholder" 
     :name="name" />
    <span
      class="error-text"
      v-if="name == 'kana'" 
      v-show="kanaValidation"
      >カタカナで入力してください。</span
    >
    <span
      class="error-text"
      v-else-if="name == 'mail'"
      v-show="mailValidation"
      >正しいメール形式で入力してください。</span
    >
    <span
      class="error-text"
      v-if="!required"
      v-show="!requiredCheck"
      >必須入力項目です。</span
    >
  </div>
</template>

<script>
// import Form from '../views/Staff.vue'
export default {
    name: 'FormInputText',
    data() {
      return {
        requiredFlg: false,
        kanaFlg: false,
        mailFlg: false
      }
    },
    props:['placeholder', 'name', 'value', 'required', 'staff'],
    computed: {
      requiredCheck: function () {
           this.$emit('update-status', this.staff)
           return this.value.trim()
        },
      kanaValidation: function () {
            var kanaPattern = /^[\u30a0-\u30ff]+$/;
            var result = kanaPattern.test(this.value.trim());
            this.staff[this.name] = result;
            this.$emit('update-status', this.staff);
            return result

      },
      mailValidation: function () {
            this.value.trim()
            var mailPattern = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
            if(this.value.match(mailPattern)){
                return false
            }else{
                return true
            }
        },
    }
    // methods: {
    //     requiredCheck: function () {
    //       console.log("aaaa")
    //        this.value.trim()
    //        return this.value.length > 0 ? false :true
    //     },
    //     kanaValidation: function () {
    //         this.value.trim()
    //         var kanaPattern = /^[\u30a0-\u30ff]+$/;
    //         if(this.value.match(kanaPattern)){
    //             return false
    //         }else{
    //             return true
    //         }
    //     },
    //     mailValidation: function () {
    //         this.value.trim()
    //         var mailPattern = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
    //         if(this.value.match(mailPattern)){
    //             return false
    //         }else{
    //             return true
    //         }
    //     },
    // },
    
}

</script>

<style scoped>

</style>