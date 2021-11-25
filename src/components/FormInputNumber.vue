<template>
  <div>
    <!-- required、maxlengthだけ -->
    <input
     :value="value"
     @input="$emit('input', $event.target.value)"
     required
     :placeholder="placeholder" />
    <span
      class="error-text"
      v-show="numberValidation()"
      >数値で入力してください。</span
    >
    <span
      class="error-text"
      v-if="!required"
      v-show="requiredCheck()"
      >必須入力項目です。</span
    >
  </div>
</template>

<script>
// import Form from '../views/Staff.vue'
export default {
    name: 'FormInputNumber',
    props:['placeholder', 'name', 'value', 'required'],
    methods: {
        requiredCheck: function () {
           this.value.trim()
           return this.value.length > 0 ? false :true
        },
        numberValidation: function () {
            this.value.trim()
            var numberPattern = /^([1-9]\d*|0)$/;
            if(this.value.match(numberPattern)){
                return false
            }else{
                return true
            }
        },
    },
    
}

</script>

<style scoped>

</style>