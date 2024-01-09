<template>
  <div class="BasInfo" >
    <div
        v-for="(objct, index) in ArrCreateFormVTwo" :key="index"
        v-bind:class="takeIdNameElementKey(objct)"
    >
      <label class="lable_elemet" v-bind:for=" takeIdNameElementKey(objct) " >{{ `${objct.label}${(objct.mustBeFilledIn) ? '*' : '' } : ` }}</label>
      <b v-if="(objct.htmlType === 'input') && objct.type === 'tel'" >+7</b>
      <input
          v-if="objct.htmlType === 'input'"
          v-bind:type="objct.type"
          v-bind:id=" takeIdNameElementKey(objct) "
          v-bind:name=" takeIdNameElementKey(objct) "
          v-bind:value="() => {
            switch(objct.type) {
              case 'checkbox':
                return objct.idNameElement[this.takeIdNameElementKey(objct)];
              default:
                return '';
            }
          }"
          v-model="objct.idNameElement[ takeIdNameElementKey(objct) ]"
          :class="{ invalid: $v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].$invalid }"
          v-on:input="sendMainComponent(!$v.$invalid, ArrCreateFormVTwo)"
      >
      <select
          v-else-if="objct.htmlType === 'select'"
          v-bind:id=" takeIdNameElementKey(objct) "
          v-bind:name=" takeIdNameElementKey(objct) "
          v-bind:multiple="(objct.type === 'multiple')"
          v-model="objct.idNameElement[ takeIdNameElementKey(objct) ]"
          :class="{ invalid: $v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].$invalid }"
          v-on:change="sendMainComponent(!$v.$invalid, ArrCreateFormVTwo)"
      >
        <option
            v-for="opon in objct.auxiliaryValue" :key="opon"
            v-bind:value="opon"
        >
          {{ opon }}
        </option>
      </select>
      <!--Отладка-->
      <small v-if="false"> "{{ objct.idNameElement[ takeIdNameElementKey(objct) ] }}" </small>

      <div v-if="(objct.htmlType === 'input') && objct.type === 'tel'" >
        <b>Введённый номер: {{ leblePhoneNumber(objct) }}</b>
      </div>
      <br v-else>

      <!--Ошибки required ruText justOneWord numeric maxLength-->
      <customValidators v-bind:mainObject="$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ]"/>

    </div>
  </div>
</template>

<script>
import { required, numeric, maxLength } from 'vuelidate/lib/validators'
import { ruText, justOneWord } from './customValidators'
import customValidators from "@/components/customValidators";

let idBasInfoVTwo = 0;
let bufidBasInfoVTwo = 0;

export default {
  name: "BasicInformation",
  components: {
    customValidators
  },
  data() {
    return {
      lastValueSent: false,
      ArrCreateFormVTwo: {
        [idBasInfoVTwo++]: { idNameElement: {surname: ''}, htmlType: 'input', type: 'text', label: 'Фамилия', mustBeFilledIn: true, auxiliaryValue: '' },
        [idBasInfoVTwo++]: { idNameElement: {name: ''}, htmlType: 'input', type: 'text', label: 'Имя', mustBeFilledIn: true, auxiliaryValue: '' },
        [idBasInfoVTwo++]: { idNameElement: {middleName: ''}, htmlType: 'input', type: 'text', label: 'Отчество', mustBeFilledIn: false, auxiliaryValue: '' },
        [idBasInfoVTwo++]: { idNameElement: {dateBirth: ''}, htmlType: 'input', type: 'date', label: 'Дата рождения', mustBeFilledIn: true, auxiliaryValue: '' },
        [idBasInfoVTwo++]: { idNameElement: {phoneNumber: ''}, htmlType: 'input', type: 'tel', label: 'Номер телефона', mustBeFilledIn: true, auxiliaryValue: '' },
        [idBasInfoVTwo++]: { idNameElement: {gender: ''}, htmlType: 'input', type: 'text', label: 'Пол', mustBeFilledIn: false, auxiliaryValue: '' },
        [idBasInfoVTwo++]: { idNameElement: {clientGroup: []}, htmlType: 'select', type: 'multiple', label: 'Группа клиентов', mustBeFilledIn: true, auxiliaryValue: ['VIP', 'Проблемные', 'ОМС'] },
        [idBasInfoVTwo++]: { idNameElement: {attendingDoctor: ''}, htmlType: 'select', type: '', label: 'Лечащий врач', mustBeFilledIn: false, auxiliaryValue: ['Иванов', 'Захаров', 'Чернышева'] },
        [idBasInfoVTwo++]: { idNameElement: {notSendSMS: false}, htmlType: 'input', type: 'checkbox', label: 'Не отправлять СМС', mustBeFilledIn: false, auxiliaryValue: '' },
      },

    }
  },
  validations: {
    ArrCreateFormVTwo: {//required ruText justOneWord numeric maxLength
      [bufidBasInfoVTwo++]: { idNameElement: {surname: {required, ruText, justOneWord}} },
      [bufidBasInfoVTwo++]: { idNameElement: {name: {required, ruText, justOneWord}} },
      [bufidBasInfoVTwo++]: { idNameElement: {middleName: {ruText, justOneWord}} },
      [bufidBasInfoVTwo++]: { idNameElement: {dateBirth: {required}} },
      [bufidBasInfoVTwo++]: { idNameElement: {phoneNumber: {required, numeric, maxLength: maxLength(10)}} },
      [bufidBasInfoVTwo++]: { idNameElement: {gender: {ruText}}} ,
      [bufidBasInfoVTwo++]: { idNameElement: {clientGroup: {required}} },
      [bufidBasInfoVTwo++]: { idNameElement: {attendingDoctor: {}} },
      [bufidBasInfoVTwo++]: { idNameElement: {notSendSMS: {}} },
    },

  },
  methods: {
    takeIdNameElementKey(obj) {
      return Object.keys(obj.idNameElement)[0];
    },
    leblePhoneNumber(objct) {
      let bufValue = objct.idNameElement[ this.takeIdNameElementKey(objct) ];
      return `+7 (${bufValue.slice(0, 3)}) ${bufValue.slice(3, 6)} ${bufValue.slice(6, 8)} ${bufValue.slice(8, 10)}`
    },
    /* Для работы функции нужно объявить lastValueSent и v-on:input="sendMainComponent(!$v.$invalid, <текущий объект>)" (пример) */
    sendMainComponent(valid, obj) {
      if ( this.lastValueSent !== valid ) {
        console.log(`BasicInformation - sendMainComponent: ${valid}`);
        this.lastValueSent = valid;
        return this.$emit( 'validet', {'0': valid, 'objct': obj} )
      }
    },

  }
}
</script>

<style scoped>

</style>