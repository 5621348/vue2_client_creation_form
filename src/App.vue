<template>
  <div id="app">

    <div class="utility-application">
  <!--    <img alt="Vue logo" src="./assets/logo.png">-->
      <!--HelloWorld msg="Welcome to Your Vue.js App"/-->
      <form class="Client_Creation_Form">

        <div class="ClientInformation">
          <table class="main-table"  >
            <caption>
              <span class="Form_Content" ><h2>Создание нового клиента</h2></span>
            </caption>

            <tr>
              <td>
                <BasicInformation v-on:validet="formValidet($event)" />
              </td>
            </tr>
  <!--            <p>______________</p><br>-->
            <tr>
              <td>
                <ClientAdress v-on:validet="formValidet($event)" />
              </td>
            </tr>
  <!--            <p>______________</p><br>-->
            <tr>
              <td>
                <ClientPassport v-on:validet="formValidet($event)" />
              </td>
            </tr>

          </table>
        </div>

        <div class="temlate-submit-button">
          <button
              type="submit"
              class="submit-button"
              v-bind:title=" (validateButtonValue) ? 'Для продолжения, форма должна быть правильно заполнена ' : 'Вознестись'"
              v-bind:disabled="validateButtonValue"
              v-on:click="creatingAnObjectToSend()"
          >
            Создать
          </button>
        </div>

      </form>

      <!--button v-on:click="creatingAnObjectToSend()" >
        Создать  2
      </button-->
    </div>

  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import BasicInformation from "@/components/BasicInformation";
import ClientAdress from "@/components/ClientAdress";
import ClientPassport from "@/components/ClientPassport";


export default {
  name: 'App',
  components: {
    // HelloWorld,
    BasicInformation,
    ClientAdress,
    ClientPassport
  },
  data() {
    return {
      valideteComponents: [false, false, false],
      validateButtonValue: true,
      screenSize: 1300,

      objectsForSaveValueForm: [],
      objectsForBackend: {},
    }
  },
  methods: {
    widthScreen() {
      return window.innerWidth;
    },
    formValidet(childObj){
      console.log(childObj);
      let bufObj = Object.keys(childObj)[0];
      this.valideteComponents[ bufObj ] = childObj[ bufObj ];
      //console.log(`bufObj: ${bufObj}`);
      console.log( this.valideteComponents );
      if ( this.valideteComponents.every( (element) => element === true ) ){
        console.log('%ctestValidet: ok ', 'color: Green;');
        this.validateButtonValue = false;
      } else {
        console.log('%ctestValidet: not-ok ', 'color: Red;');
        this.validateButtonValue = true;
      }

      this.objectsForSaveValueForm[ bufObj ] = childObj.objct;
      console.log( this.objectsForSaveValueForm[ bufObj ] );
    },
    creatingAnObjectToSend() {
      for ( let i = 0; i < this.objectsForSaveValueForm.length; i++ ) {
        console.log(  this.objectsForSaveValueForm );
        for ( let elem in this.objectsForSaveValueForm[i] ) {
          const buff = this.objectsForSaveValueForm[i][elem];
          this.objectsForBackend[ this.takeIdNameElementKey( buff ) ] = this.objectsForSaveValueForm[i][elem].idNameElement[ this.takeIdNameElementKey( buff ) ]
          // console.log( this.objectsForBackend );
        }
      }
      console.log( JSON.stringify(this.objectsForBackend) );  //отправляем на бэк

    },
    takeIdNameElementKey(obj) {
      return Object.keys(obj.idNameElement)[0];
    },
  }
}


</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #9b9fff;
  margin: 2.5% 8%;
}

body {
  background-image: url( './assets/registrationBackground.jpg');
  background-size: cover;

}

.utility-application {
  background-color: rgba(0%, 0%, 0%, 90%);
}

.Form_Content {
  color: #cccccc;
}

.main-table {
  margin: auto;
  width: 33%;

  /*border: groove;*/
}

.main-table * {
    clear:both;
    text-align:right;
    line-height:25px;
}

.lable_elemet {
  float:left;
  /*color: #00FF00;*/
}

input, select {
  text-align: left;
  color: #cccccc;
  background-color: #2c3e50;
  margin-top: 6px;
}

.Client_Creation_Form {
  text-align: center;
}

.submit-button {
  background-color: v-bind( (validateButtonValue) ? 'black' : '#00FF00' );
  border: none;
  /*color: white;*/
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 6px 2px;
  cursor: v-bind( (validateButtonValue) ? 'not-allowed' : 'pointer' );
  opacity: v-bind( (validateButtonValue) ? '0.1' : '1' );
}

.invalid {
  border: 2px solid darkorange;
}

.text_invalid {
  text-align: right;
  color: darkorange;
}
</style>
