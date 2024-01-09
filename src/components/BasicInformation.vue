<template>
  <div class="BasInfo" >
    <!--<div>
      <input
          id="fname"
          type="text"
      >
      <label for="fname">Фамилия</label>
      <small>sraka</small>
    </div>
    <div>
      <input
          id="sname"
          type="text"
      >
      <label for="sname">Имя</label>
      <small>sraka</small>
    </div>
    <div>
      <input
          id="lname"
          type="text"
      >
      <label for="lname">Отчество</label>
      <small>a</small>
    </div>
    <div>
      <input
          id="DateBirth"
          type="text"
      >
      <label for="DateBirth">Дата рождения</label>
      <small>sraka</small>
    </div>
    <div>
      <input
          id="PhoneNumber"
          type="text"
      >
      <label for="PhoneNumber">Номер телефона</label>
      <small>sraka</small>
    </div>
    <div>
      <input
          id="Gender"
          type="text"
      >
      <label for="Gender">Пол</label>
      <small>a</small>
    </div>
    <div>
      <input
          id="ClientGroup"
          type="text"
      >
      <label for="ClientGroup">Группа клиентов</label>
      <small>sraka</small>
    </div> -->

    <!--input
        type="text" v-model="userForm.mobile" id="mobile" name="mobile" class="form-control"
        :class="{ 'is-invalid': $v.userForm.mobile.$error }"
    >
    <div-- v-for="(el, index) in ArrCreateFormVTwo" :key="el" >
      <input
          type="text"
          id="mobile"
          name="mobile"
          class="form-control"
          v-model="ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(index) ]"
          :class="{ invalid: $v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(index) ].$invalid }"
      >
      <small v-if="$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(index) ].$invalid"> SRAKA </small>
      <br>{{ el }} {{ index.replace(regexid, '') }}  {{ Object.keys(ArrCreateFormVTwo[index].idNameElement)[0] }}
      <br>sraka: {{ takeIdNameElementKey(index) }}
    </div-->

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
              // case 'tel':
              //   return '+7';
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

      <!--div v-for="(reaction, indexReaction) in arrReactionsToIncorrectInput" :key="indexReaction" >
        <small
            class="text_invalid"
            v-if="($v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ][reaction.typeValidation] !== undefined) &&
                  (!$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ][reaction.typeValidation]) &&
                  (indexReaction === 0)"
        >
          {{ reaction.textValidation }}
        </small>
        <small
            class="text_invalid"
            v-else-if="($v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ][reaction.typeValidation] !== undefined) &&
                       (!$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ][reaction.typeValidation])"
        >
          {{ reaction.textValidation }}
        </small>
      </div-->

      <!-- <small
        class="text_invalid"
        v-if="($v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].required !== undefined) &&
              (!$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].required)"
      >
        Имеется ошибка! 1
      </small>
      <small
          class="text_invalid"
          v-else-if="($v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].ruText !== undefined) &&
                     (!$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].ruText)"
      >
        Имеется ошибка! 2
      </small>
      <small
          class="text_invalid"
          v-else-if="($v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].justOneWord !== undefined) &&
                     (!$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].justOneWord)"
      >
        Имеется ошибка! 3
      </small>
      <small
          class="text_invalid"
          v-else-if="($v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].numeric !== undefined) &&
                     (!$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].numeric)"
      >
        Имеется ошибка! 4
      </small>
      <small
        class="text_invalid"
        v-else-if="($v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].maxLength !== undefined) &&
                   (!$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ].maxLength)"
        >
        Имеется ошибка! 5
      </small> -->
    </div>

      <!--
    <div v-for="div in ArrCreateForm" :key="div.id" >
      <label v-bind:for="div.idNameElement" >{{ `${div.label}${(div.mustBeFilledIn) ? '*' : '' } : ` }}</label>
      <input
          v-if="div.htmlType === 'input'"

          class="form-control"
          v-bind:id="div.idNameElement"
          v-bind:type="div.type"
          v-bind:name="div.idNameElement"
          v-bind:value="(div.type === 'checkbox' && div.value === '') ? div.value=false : ''"
          v-model="div.value"
          @input="$v.div.idNameElement.$touch()"
      >-->
<!--      :class="{invalid: (/*$v.surname.$dirty &&*/ !$v.div.value.required)}"-->

      <!--select
          v-else-if="div.htmlType === 'select'"
          v-bind:multiple="(div.type === 'multiple')"
          v-model="div.setValue"
      >
        <option
            v-for="opon in div.value" :key="opon"
            v-bind:value="opon"
        >
          {{ opon }}
        </option>
      </select--><!--
      <small v-if="div.htmlType ==='input'"> {{ div.value }} </small>
      <small v-else > {{ div.setValue }}</small>

      <br>
      <small
        class="text_invalid"
        v-if="(/*$v.surname.$dirty &&*/ !$v.surname.required)"
      >
        срака {{ div.mustBeFilledIn }}
      </small>
    </div>
-->

  </div>
</template>

<script>
import { required, numeric, maxLength,/* alpha, minLength*/ } from 'vuelidate/lib/validators'
import {ruText, justOneWord} from './customValidators'
import customValidators from "@/components/customValidators";

let idBasInfo = 0;
let idBasInfoVTwo = 0;
let bufidBasInfoVTwo = 0;

// const ruText = (value) => value.search(/[А-я]+/) !== -1 ;
// const justOneWord = (value) => value.search(/([А-яA-z]+)\s+([А-яA-z]+)/) === -1 ;

export default {
  name: "BasicInformation",
  components: {
    customValidators
  },
  data() {
    return {
      regexid: /[A-Za-z]*_*/,//'id_'+index.replace(regexid, '')
      lastValueSent: false,
      screen: '',
      ArrCreateForm: [
        { id: idBasInfo++, htmlType: 'input', idNameElement: 'surname', type: 'text', value: '', label: 'Фамилия', conditions: '', mustBeFilledIn: true },
        { id: idBasInfo++, htmlType: 'input', idNameElement: 'name', type: 'text', value: '', label: 'Имя', conditions: '', mustBeFilledIn: true },
        { id: idBasInfo++, htmlType: 'input', idNameElement: 'middleName', type: 'text', value: '', label: 'Отчество', conditions: '', mustBeFilledIn: false },
        { id: idBasInfo++, htmlType: 'input', idNameElement: 'dateBirth', type: 'date', value: '', label: 'Дата рождения', conditions: '', mustBeFilledIn: true },
        { id: idBasInfo++, htmlType: 'input', idNameElement: 'phoneNumber', type: 'tel', value: '', label: 'Номер телефона', conditions: '', mustBeFilledIn: true },
        { id: idBasInfo++, htmlType: 'input', idNameElement: 'gender', type: 'text', value: '', label: 'Пол', conditions: '', mustBeFilledIn: false },
        { id: idBasInfo++, htmlType: 'select',  idNameElement: 'clientGroup', type: 'multiple', value: ['VIP', 'Проблемные', 'ОМС'], setValue: '', label: 'Группа клиентов', conditions: '', mustBeFilledIn: true },
        { id: idBasInfo++, htmlType: 'select',  idNameElement: 'attendingDoctor', type: '', value: ['Иванов', 'Захаров', 'Чернышева'], setValue: '', label: 'Лечащий врач', conditions: '', mustBeFilledIn: false },
        { id: idBasInfo++, htmlType: 'input', idNameElement: 'notSendSMS', type: 'checkbox', value: '', label: 'Не отправлять СМС', conditions: '', mustBeFilledIn: false },
      ],
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
      // userForm: {
      //   mobile: ''
      // },
      // userFormTest: {
      //   id_0: {
      //     htmlType: 'input',
      //     idNameElement: {surname: ''}
      //   }
      // }
      // surname: ''
    }
  },
  validations: {
    /* ArrCreateForm: [
      {idNameElement: {required}}
    ],
    surname: {required},// alpha},
    password: {required, minLength: minLength(6)},*/
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
    // userForm: {
    //   mobile: {
    //     required
    //   }
    // },
    // userFormTest: {
    //   id_0: {
    //     idNameElement: {surname: {required}}
    //   }
    // }
  },
  methods: {
    takeIdNameElementKey(obj) {
      // obj.htmlType === 'select' ? console.log(obj) : '';
      // return Object.keys(this.ArrCreateFormVTwo[index].idNameElement)[0];
      // console.log('ArrCreateFormVTwo[5]')
      // console.log(this.ArrCreateFormVTwo[5].idNameElement.gender.search(/[А-я]*/))
      return Object.keys(obj.idNameElement)[0];
    },
    leblePhoneNumber(objct) {
      let bufValue = objct.idNameElement[ this.takeIdNameElementKey(objct) ];
      return `+7 (${bufValue.slice(0, 3)}) ${bufValue.slice(3, 6)} ${bufValue.slice(6, 8)} ${bufValue.slice(8, 10)}`
    },
    /* Для работы функции нужно объявить lastValueSent и v-on:input="sendMainComponent(!$v.$invalid)" (пример) */
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