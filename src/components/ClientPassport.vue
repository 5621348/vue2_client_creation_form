<template>
  <div class="ClientPassport" >
    <div
        v-for="(pasp, index) in ArrPassport" :key="index"
        v-bind:class="takeIdNameElementKey(pasp)"
    >
      <label class="lable_elemet" v-bind:for="takeIdNameElementKey(pasp)" >{{ `${pasp.label}${(pasp.mustBeFilledIn) ? '*' : '' } : ` }}</label>
      <input
          v-if="pasp.htmlType === 'input'"
          v-bind:type="pasp.type"
          v-bind:id=" takeIdNameElementKey(pasp) "
          v-bind:name=" takeIdNameElementKey(pasp) "
          v-bind:value="() => {
            switch(pasp.type) {
              case 'checkbox':
                return pasp.idNameElement[ takeIdNameElementKey(pasp) ];
              default:
                return '';
            }
          }"
          v-model="pasp.idNameElement[ takeIdNameElementKey(pasp) ]"
          :class="{ invalid: $v.ArrPassport[index].idNameElement[ takeIdNameElementKey(pasp) ].$invalid }"
          v-on:input="sendMainComponent(!$v.$invalid, ArrPassport)"
      >
      <select
          v-else-if="pasp.htmlType === 'select'"
          v-bind:id=" takeIdNameElementKey(pasp) "
          v-bind:name=" takeIdNameElementKey(pasp) "
          v-bind:multiple="(pasp.type === 'multiple')"
          v-model="pasp.idNameElement[ takeIdNameElementKey(pasp) ]"
          :class="{ invalid: $v.ArrPassport[index].idNameElement[ takeIdNameElementKey(pasp) ].$invalid }"
          v-on:input="sendMainComponent(!$v.$invalid, ArrPassport)"
      >
        <option
            v-for="opon in pasp.auxiliaryValue" :key="opon"
            v-bind:value="opon"
        >
          {{ opon }}
        </option>
      </select>
      <!--Отладка-->
      <small v-if="false"> "{{ pasp.idNameElement[ takeIdNameElementKey(pasp) ] }}" </small>

      <!--Ошибки required ruText justOneWord numeric maxLength-->
      <customValidators v-bind:mainObject="$v.ArrPassport[index].idNameElement[ takeIdNameElementKey(pasp) ]"/>

    </div>
  </div>
</template>

<script>
import customValidators from "@/components/customValidators";
import { required, numeric } from "vuelidate/lib/validators";
import { ruenText } from "@/components/customValidators";

let idPaspInfo = 0;
let bufidPaspInfo = 0;

export default {
  name: "ClientPassport",
  components: {
    customValidators
  },
  data() {
    return {
      lastValueSent: false,
      ArrPassport: {
        [idPaspInfo++]: { idNameElement: {typePasp: ''}, htmlType: 'select', type: '', label: 'Тип документа', mustBeFilledIn: true, auxiliaryValue: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'], },
        [idPaspInfo++]: { idNameElement: {dateIssue: ''}, htmlType: 'input', type: 'date', label: 'Дата выдачи', mustBeFilledIn: true, auxiliaryValue: '', },
        [idPaspInfo++]: { idNameElement: {series: ''}, htmlType: 'input', type: 'text', label: 'Серия', mustBeFilledIn: false, auxiliaryValue: '', },
        [idPaspInfo++]: { idNameElement: {number: ''}, htmlType: 'input', type: 'text', label: 'Номер', mustBeFilledIn: false, auxiliaryValue: '', },
        [idPaspInfo++]: { idNameElement: {issuedBy: ''}, htmlType: 'input', type: 'text', label: 'Кем выдан', mustBeFilledIn: false, auxiliaryValue: '', },
      }
    }

  },
  validations: {
    ArrPassport: {
      [bufidPaspInfo++]: { idNameElement: {typePasp: {required}} },
      [bufidPaspInfo++]: { idNameElement: {dateIssue: {required}} },
      [bufidPaspInfo++]: { idNameElement: {series: {numeric}} },
      [bufidPaspInfo++]: { idNameElement: {number: {numeric}} },
      [bufidPaspInfo++]: { idNameElement: {issuedBy: {ruenText}} },
    }
  },
  methods: {
    takeIdNameElementKey(obj) {
      return Object.keys(obj.idNameElement)[0];
    },
    /* Для работы функции нужно объявить lastValueSent и v-on:input="sendMainComponent(!$v.$invalid, <текущий объект>)" (пример) */
    sendMainComponent(valid, obj) {
      if ( this.lastValueSent !== valid ) {
        console.log(`ClientPassport - sendMainComponent: ${valid}`);
        this.lastValueSent = valid;
        return this.$emit( 'validet', {'2': valid, 'objct': obj} )
      }
    },
  }
}
</script>

<style scoped>

</style>