<template lang="pug">
  .notification
    h1.subtitle Pago Unico
      span.icon(style="margin-left: 1em")
        i.fas.fa-money-bill-alt

    .column.is-12-desktop.is-12-mobile
      .columns.is-multiline.is-mobile
        //- Futuro
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey Futuro
            .card-content
              b-input(
                placeholder="Futuro"
                type="number"
                min="0"
                v-model="futuro"
              )

        //- Presente
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey Presente
            .card-content
              b-input(
                placeholder="Presente"
                type="number"
                min="0"
                v-model="presente"
              )

        //- Interes
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey Interes
            .card-content
              b-input(
                placeholder="Interes"
                type="number"
                min="0"
                v-model="interes"
              ) 

        //- Tiempo
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey Tiempo
            .card-content
              b-input(
                placeholder="Tiempo"
                type="number"
                min="0"
                v-model="tiempo"
              )

    p ¿Capitalizable? 
      span.icon
       i.fas.fa-file-invoice-dollar
      b-switch(
        type="is-success"
        v-model="switchCapitalizable"
        true-value="Si"
        false-value="No"
      ) {{ switchCapitalizable }}

    .block(v-if="this.switchCapitalizable == 'Si'" style="margin-bottom: 2em")     
      b-radio(
        v-model="radio"
        native-value = 1
      ) Anual

      b-radio(
        v-model="radio"
        native-value = 2
      ) Semestral

      b-radio(
        v-model="radio"
        native-value = 3
      ) Cuatrimestral

      b-radio(
        v-model="radio"
        native-value = 4
      ) Trimestral
      
      b-radio(
        v-model="radio"
        native-value = 12
      ) Mensualmente

      b-radio(
        v-model="radio"
        native-value = 24
      ) Quincenalmente

      b-radio(
        v-model="radio"
        native-value = 52
      ) Semanalmente

      b-radio(
        v-model="radio"
        native-value = 360
      ) Diariamente
    
    div

    p(style="margin-top: 2em") ¿Calcular Interes? 
      b-switch(
        type="is-success"
        v-model="switchInteres"
        true-value="Si"
        false-value="No"
        :click="CalcularInteres()"
      ) {{ switchInteres }}

    p ¿Calcular Tiempo? 
      b-switch(
        type="is-success"
        v-model="switchTiempo"
        true-value="Si"
        false-value="No"
        :click="CalcularTiempo()"
      ) {{ switchTiempo }}

    p ¿Calcular Futuro? 
      b-switch(
        type="is-success"
        v-model="switchFuturo"
        true-value="Si"
        false-value="No"
        :click="CalcularFuturo()"
      ) {{ switchFuturo }}

    p ¿Calcular Presente? 
      b-switch(
        type="is-success"
        v-model="switchPresente"
        true-value="Si"
        false-value="No"
        :click="CalcularPresente()"
      ) {{ switchPresente }}

    p(v-if="this.switchInteres == 'Si' && this.switchTiempo == 'No' && this.switchFuturo == 'No' && this.switchPresente == 'No' ") Interes: {{ interes }}%
    p(v-if="this.switchInteres == 'No' && this.switchTiempo == 'Si' && this.switchFuturo == 'No' && this.switchPresente == 'No' ") Tiempo: {{ tiempo }}
    p(v-if="this.switchInteres == 'No' && this.switchTiempo == 'No' && this.switchFuturo == 'Si' && this.switchPresente == 'No' ") Futuro: {{ futuro }}
    p(v-if="this.switchInteres == 'No' && this.switchTiempo == 'No' && this.switchFuturo == 'No' && this.switchPresente == 'Si' ") Presente: {{ presente }}

</template>

<script>

export default {
  name: 'Calculadora',
  data: () => ({
    futuro: 0, // F
    presente: 0, // P
    interes: 0, // i
    tiempo: 0, // n

    switchInteres: 'No',
    switchTiempo: 'No',
    switchFuturo: 'No',
    switchPresente: 'No',

    radio: 0,
    switchCapitalizable: 'No',
  }),
  methods: {
    CalcularMathPow(interes, tiempo){
      return Math.pow((1 + interes), tiempo) 
    },
    CalcularInteres () {
      if (this.interes == 0 && this.switchInteres == 'Si' && this.switchTiempo == 'No' && this.switchFuturo == 'No') {
        this.interes = Math.pow( this.futuro / this.presente, 1 / this.tiempo) - 1
        this.interes = this.interes * 100
      }
    },
    CalcularTiempo () {
      if ( this.tiempo == 0 &&  this.interes > 0 &&  this.presente > 0 && this.switchTiempo == "Si" && this.switchInteres == 'No' && this.switchFuturo == 'No') {
        let interesNeto = 0
        if(this.switchCapitalizable == 'No'){
          interesNeto = this.interes / 100
        } else {
          interesNeto = this.Capitalizable(this.interes, this.radio) 
        }
        this.tiempo = Math.log(this.futuro / this.presente) / Math.log(1 + interesNeto)
      }
    },
    CalcularFuturo () {
      if (this.presente > 0 &&  this.interes > 0 && this.switchTiempo == "No" && this.switchInteres == 'No' && this.switchFuturo == 'Si') {
        let interesNeto = 0
        if(this.switchCapitalizable == 'No'){
          interesNeto = this.interes / 100
        } else {
          interesNeto = this.Capitalizable(this.interes, this.radio)
        }
        let adentro =  Math.pow( 1 + interesNeto, this.tiempo ) 
        this.futuro = this.presente * adentro
      }
    },
    CalcularPresente () {
      if ( this.futuro > 0 && this.interes > 0 && this.tiempo > 0 && this.switchTiempo == "No" && this.switchInteres == 'No' && this.switchFuturo == 'No' && this.switchPresente == 'Si') {
        let interesNeto = 0
        if(this.switchCapitalizable == 'No'){
          interesNeto = this.interes / 100
        } else {
          interesNeto = this.Capitalizable(this.interes, this.radio)
        }
        this.presente = this.futuro / this.CalcularMathPow(interesNeto, this.tiempo)
      }
    },
    Capitalizable ( interesSimple, tiempo ){
      let resultado = 0
      return resultado = this.CalcularMathPow( (interesSimple / tiempo ) / 100, this.radio) - 1
    }
  }
}
</script>

<style lang="sass" scoped>
</style>
