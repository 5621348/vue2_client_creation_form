<!--Для использования этого компонента нужно:
  1. import customValidators from "@/components/customValidators";
  2. export default {
      components: {
        customValidators
      }
     }
  3. <customValidators v-bind:mainObject="$v.ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) ]"/>
  (ArrCreateFormVTwo[index].idNameElement[ takeIdNameElementKey(objct) - Пример объекта, валидацию которого нужно отобразить)
-->

<template>
  <div>
    <!--Для отладки-->
    <i v-if="!true">{{ mainObject }}</i>

    <div v-for="(reaction, indexReaction) in arrReactionsToIncorrectInput" :key="indexReaction" >
      <small
          class="text_invalid"
          v-if="(mainObject[reaction.typeValidation] !== undefined) &&
                  (!mainObject[reaction.typeValidation]) &&
                  (indexReaction === 0)"
      >
        {{ reaction.textValidation }}
      </small>
      <small
          class="text_invalid"
          v-else-if="(mainObject[reaction.typeValidation] !== undefined) &&
                       (!mainObject[reaction.typeValidation])"
      >
        {{ reaction.textValidation }}
      </small>
    </div>

  </div>
</template>

<script>
//Регурка отрицает нужное нам значение, чтобы vuelidate показывал ошибку только в случае наличия символов
export const ruText = (value) => value.search(/[^А-яё\s]+/) === -1 ;
export const enText = (value) => value.search(/[^A-z'\s]+/) === -1 ;
export const ruenText = (value) => value.search(/[^А-яёA-z'\s]+/) === -1 ;

export const justOneWord = (value) => value.search(/([А-яёA-z]+[-+А-яёA-z]+)\s+([А-яёA-z]+)/) === -1 ;

// export const numbersWithSigns = (value) => value.search( /[^0-9]+|[^0-9]+[^(К)]{1}[^0-9]+/ ) === -1 ;
// export const startRuPhone = (value) => value.search(/(?:\+7)/) === -1 ;


export default {
  name: "customValidators",
  props: {
    mainObject: {
      type: Object
    }
  },
  data() {
    return {
      arrReactionsToIncorrectInput: [
        {typeValidation: 'ruText', textValidation: 'Поле принимает символы русского алфавита '},
        {typeValidation: 'enText', textValidation: 'Поле принимает символы латинского алфавита '},
        {typeValidation: 'ruenText', textValidation: 'Поле принимает символы русского или латинского алфавита '},
        {typeValidation: 'justOneWord', textValidation: 'Поле должнно содержать только одно слово '},
        {typeValidation: 'numbersWithSigns', textValidation: 'Поле должнно содержать цифры, ожидалось "цифра(ы)" или "цифра(ы)Кцифра(ы)"'},

        {typeValidation: 'required', textValidation: 'Поле не должнно быть пустым '},
        {typeValidation: 'numeric', textValidation: 'Поле должнно содержать только цифры '},
        {typeValidation: 'maxLength', textValidation: 'Поле не должнно превышать 10 символов '},
        {typeValidation: 'alpha', textValidation: 'Поле принимает символы латинского алфавита '},
      ]
    }
  }

}
</script>

<!--<style scoped>-->

<!--</style>-->