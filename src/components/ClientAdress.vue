<template>
  <div class="ClientAdress" >
    <div
        v-for="(adr, index) in ArrAdress" :key="index"
        v-bind:class="takeIdNameElementKey(adr)"
    >
      <label class="lable_elemet" v-bind:for="takeIdNameElementKey(adr)" >{{ `${adr.label}${(adr.mustBeFilledIn) ? '*' : '' } : ` }}</label>
      <input
          v-if="adr.htmlType === 'input'"
          v-bind:type="adr.type"
          v-bind:id=" takeIdNameElementKey(adr) "
          v-bind:name=" takeIdNameElementKey(adr) "
          v-bind:value="() => {
            switch(adr.type) {
              case 'checkbox':
                return adr.idNameElement[ takeIdNameElementKey(adr) ];
              default:
                return '';
            }
          }"
          v-model="adr.idNameElement[ takeIdNameElementKey(adr) ]"
          :class="{ invalid: $v.ArrAdress[index].idNameElement[ takeIdNameElementKey(adr) ].$invalid }"
          v-on:input="sendMainComponent(!$v.$invalid, ArrAdress)"
      >
      <select
          v-else-if="adr.htmlType === 'select'"
          v-bind:id=" takeIdNameElementKey(adr) "
          v-bind:name=" takeIdNameElementKey(adr) "
          v-bind:multiple="(adr.type === 'multiple')"
          v-model="adr.idNameElement[ takeIdNameElementKey(adr) ]"
          :class="{ invalid: $v.ArrAdress[index].idNameElement[ takeIdNameElementKey(adr) ].$invalid }"
          v-on:input="sendMainComponent(!$v.$invalid, ArrAdress)"
      >
        <option
            v-for="opon in adr.auxiliaryValue" :key="opon"
            v-bind:value="opon"
        >
          {{ opon }}
        </option>
      </select>
      <!--Отладка-->
      <small v-if="false"> "{{ adr.idNameElement[ takeIdNameElementKey(adr) ] }}" </small>

      <!--Ошибки required ruText justOneWord numeric maxLength-->
      <customValidators v-bind:mainObject="$v.ArrAdress[index].idNameElement[ takeIdNameElementKey(adr) ]"/>

    </div>
  </div>
</template>

<script>
import { required, numeric } from 'vuelidate/lib/validators'
import { ruenText, ruText } from "@/components/customValidators";
import customValidators from "@/components/customValidators";

let idAdrInfo = 0;
let bufidAdrInfo = 0;

export default {
  name: "ClientAdress",
  components: {
    customValidators
  },
  data() {
    return {
      ArrAdress: {
        [idAdrInfo++]: { idNameElement: {city: ''}, htmlType: 'input', type: 'text', label: 'Город', mustBeFilledIn: true, auxiliaryValue: '' },
        [idAdrInfo++]: { idNameElement: {country: ''}, htmlType: 'input', type: 'text', label: 'Страна', mustBeFilledIn: false, auxiliaryValue: '' },
        [idAdrInfo++]: { idNameElement: {region: ''}, htmlType: 'input', type: 'text', label: 'Область', mustBeFilledIn: false, auxiliaryValue: '' },
        [idAdrInfo++]: { idNameElement: {street: ''}, htmlType: 'input', type: 'text', label: 'Улица', mustBeFilledIn: false, auxiliaryValue: '' },
        [idAdrInfo++]: { idNameElement: {house: ''}, htmlType: 'input', type: 'text', label: 'Дом', mustBeFilledIn: false, auxiliaryValue: '' },
        [idAdrInfo++]: { idNameElement: {index: ''}, htmlType: 'input', type: 'text', label: 'Индекс', mustBeFilledIn: false, auxiliaryValue: '' },
      },
      lastValueSent: false,
    }
  },
  validations: {
    ArrAdress: {
      [bufidAdrInfo++]: { idNameElement: {city: {required, ruText}} },
      [bufidAdrInfo++]: { idNameElement: {country: {ruenText}} },
      [bufidAdrInfo++]: { idNameElement: {region: {ruenText}} },
      [bufidAdrInfo++]: { idNameElement: {street: {ruenText}} },
      [bufidAdrInfo++]: { idNameElement: {house: {numeric}} },
      [bufidAdrInfo++]: { idNameElement: {index: {numeric}} },
    }
  },
  methods: {
    takeIdNameElementKey(obj) {
      return Object.keys(obj.idNameElement)[0];
    },
    /* Для работы функции нужно объявить lastValueSent и v-on:input="sendMainComponent(!$v.$invalid, <текущий объект>)" (пример) */
    sendMainComponent(valid, obj) {
      console.log( obj );

      if ( this.lastValueSent !== valid ) {
        console.log(`ClientAdress - sendMainComponent: ${valid}`);
        this.lastValueSent = valid;
        return this.$emit( 'validet', {'1': valid, 'objct': obj} )
      }
    },

  }
}
</script>

<style scoped>

</style>