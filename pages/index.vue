<template lang="pug">
  .notification
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

    p ¿Calcular Interes? 
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

    p ¿Capitalizable? 
      b-switch(
        type="is-success"
        v-model="switchCapitalizable"
        true-value="Si"
        false-value="No"
        :click="CalcularFuturo()"
      ) {{ switchCapitalizable }}

    .block(v-if="this.switchCapitalizable == 'Si'")     
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

    p(v-if="this.switchInteres == 'Si' && this.switchTiempo == 'No' && this.switchFuturo == 'No'") Interes: {{ interes }}
    p {{ radio }}
</template>

<script>
import BLogo from '@/components/Logo'

export default {
  name: 'Calculadora',
  components: {
    BLogo
  },
  data: () => ({
    futuro: 0, // F
    presente: 0, // P
    interes: 0, // i
    tiempo: 0, // n

    switchInteres: 'No',
    switchTiempo: 'No',
    switchFuturo: 'No',
    switchPresente: 'No',

    radio: '',
    switchCapitalizable: 'No',
  }),
  methods: {
    CalcularInteres () {
      if (this.interes == 0 && this.switchInteres == 'Si' && this.switchTiempo == 'No') {
        this.interes = Math.pow( this.futuro / this.presente, 1 / this.tiempo) - 1
        this.interes = this.interes * 100 +'%'
      }
    },
    CalcularTiempo () {
      if (this.tiempo == 0 && this.switchTiempo == "Si" && this.switchInteres == 'No') {
      }
    },
    CalcularFuturo () {
      if (this.presente > 0 && this.switchTiempo == "No" && this.switchInteres == 'No' && this.switchFuturo == 'Si') {
        let interesNeto = this.interes / 100
        let temporal = this.presente * ( 1 + interesNeto ) 
        this.futuro = Math.pow( temporal, this.tiempo )
      }
    },
    CalcularPresente () {
      if (this.futuro > 0 && this.switchTiempo == "No" && this.switchInteres == 'No' && this.switchFuturo == 'No' && this.switchPresente == 'Si') {
        let interesNeto = this.interes / 100
        this.presente = this.futuro / Math.pow(( 1 + interesNeto ), this.tiempo)
      }
    }
  }
}
</script>

<style lang="sass" scoped>
</style>
