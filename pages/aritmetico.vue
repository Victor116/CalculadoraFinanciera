<template lang="pug">
  .notification
    h1.subtitle Gradiente Aritmetico
      span.icon(style="margin-left: 1em")
        i.fas.fa-chart-bar

    .column.is-12-desktop.is-12-mobile
      .columns.is-multiline.is-mobile
        //- AA
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey AA
            .card-content
              b-input(
                placeholder="AA"
                type="number"
                min="0"
                v-model="AA"
              )

        //- G
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey G
            .card-content
              b-input(
                placeholder="G"
                type="number"
                min="0"
                v-model="G"
              )

        //- PA
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey PA
            .card-content
              b-input(
                placeholder="PA"
                type="number"
                min="0"
                v-model="PA"
              ) 

        //- PG
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey PG
            .card-content
              b-input(
                placeholder="PG"
                type="number"
                min="0"
                v-model="PG"
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

        //- M # de periodos
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey M # de periodos
            .card-content
              b-input(
                placeholder="M # de periodos"
                type="number"
                min="0"
                v-model="radio"
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

    p(style="margin-top: 2em") ¿Calcular Interes Compuesto (ia)? 
      b-switch(
        type="is-success"
        v-model="switchInteresCompuesto"
        true-value="Si"
        false-value="No"
        :click="CalcularInteresCompuesto()"
      ) {{ switchInteresCompuesto }}

    p ¿Calcular PA? 
      b-switch(
        type="is-success"
        v-model="switchPA"
        true-value="Si"
        false-value="No"
        :click="CalcularPA()"
      ) {{ switchPA }}

    p ¿Calcular PG? 
      b-switch(
        type="is-success"
        v-model="switchPG"
        true-value="Si"
        false-value="No"
        :click="CalcularPG()"
      ) {{ switchPG }}

    p ¿Calcular PT? 
      b-switch(
        type="is-success"
        v-model="switchPT"
        true-value="Si"
        false-value="No"
        :click="CalcularPT()"
      ) {{ switchPT }}

    //- p(v-if="this.switchInteres == 'Si' && this.switchTiempo == 'No' && this.switchFuturo == 'No' && this.switchPresente == 'No' ") Interes: {{ interes }}%
    //- p(v-if="this.switchInteres == 'No' && this.switchTiempo == 'Si' && this.switchFuturo == 'No' && this.switchPresente == 'No' ") Tiempo: {{ tiempo }}
    //- p(v-if="this.switchInteres == 'No' && this.switchTiempo == 'No' && this.switchFuturo == 'Si' && this.switchPresente == 'No' ") Futuro: {{ futuro }}
    //- p(v-if="this.switchInteres == 'No' && this.switchTiempo == 'No' && this.switchFuturo == 'No' && this.switchPresente == 'Si' ") Presente: {{ presente }}

</template>

<script>
export default {
  name: 'Calculadora',
  data: () => ({
    AA: 0,
    G: 0,
    PA: 0,
    PG: 0, // p
    interes: 0, // i
    tiempo: 0, // n

    switchPA: 'No',
    switchPG: 'No',
    switchInteresCompuesto: 'No',
    switchPT: 'No',

    radio: 0, // m
    switchCapitalizable: 'No',
  }),
  methods: {
    CalcularMathPow(interes, tiempo){
      return Math.pow((1 + interes), tiempo) 
    },
    CalcularPA  () {
      if(this.tiempo > 0 && this.interes > 0 && this.AA > 0 && this.switchPA == 'Si' && this.switchPG == 'No' && this.switchInteresCompuesto == 'No' && this.switchPT == 'No' ){
        let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
          interesNeto = this.Capitalizable(this.interes, this.radio)

        let arriba = this.CalcularMathPow(interesNeto, this.tiempo) - 1
        let abajo = interesNeto * (arriba + 1)
        this.PA = this.AA * (arriba / abajo )
      }
    },
    CalcularPG  () {
      if( this.G > 0 && this.interes > 0 && this.tiempo > 0 && this.switchPA == 'No' && this.switchPG == 'Si' && this.switchInteresCompuesto == 'No' && this.switchPT == 'No' ){
        let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
          interesNeto = this.Capitalizable(this.interes, this.radio)

        let interesConTiempo = this.CalcularMathPow(interesNeto, this.tiempo)

        let top1 = interesConTiempo -1
        let bot1 = interesNeto * interesConTiempo
        let left = top1 / bot1

        let right = this.tiempo / interesConTiempo

        let adentro = left - right

        let afuera = this.G / interesNeto

        this.PG = afuera * adentro
        console.log(this.PG)
      }
    },
    CalcularInteresCompuesto  () {
      if( this.switchPA == 'No' &&
          this.switchPG == 'No' &&
          this.switchInteresCompuesto == 'No' &&
          this.switchPT == 'No' ){

      }
    },
    CalcularPT  () {
      if( this.switchPA == 'No' &&
          this.switchPG == 'No' &&
          this.switchInteresCompuesto == 'No' &&
          this.switchPT == 'No' ){

      }
    },
    // CalcularInteres () {
    //   if (this.interes == 0 && this.switchInteres == 'Si' && this.switchTiempo == 'No' && this.switchFuturo == 'No') {
    //     this.interes = Math.pow( this.futuro / this.presente, 1 / this.tiempo) - 1
    //     this.interes = this.interes * 100
    //   }
    // },
    // CalcularTiempo () {
    //   if ( this.tiempo == 0 && this.switchTiempo == "Si" && this.switchInteres == 'No' && this.switchFuturo == 'No') {
    //     let interesNeto = 0
    //     if(this.switchCapitalizable == 'No'){
    //       interesNeto = this.interes / 100
    //     } else {
    //       interesNeto = this.Capitalizable(this.interes, this.radio)
    //       console.log(interesNeto)
    //     }
    //     this.tiempo = Math.log(this.futuro / this.presente) / Math.log(1 + interesNeto)
    //   }
    // },
    // CalcularFuturo () {
    //   if (this.presente == 0 && this.switchTiempo == "No" && this.switchInteres == 'No' && this.switchFuturo == 'Si') {
    //     let interesNeto = this.interes / 100
    //     let temporal = this.presente * ( 1 + interesNeto ) 
    //     this.futuro = Math.pow( temporal, this.tiempo )
    //   }
    // },
    // CalcularPresente () {
    //   if ( this.futuro == 0 && this.switchTiempo == "No" && this.switchInteres == 'No' && this.switchFuturo == 'No' && this.switchPresente == 'Si') {
    //     let interesNeto = this.interes / 100
    //     this.presente = this.futuro / Math.pow(( 1 + interesNeto ), this.tiempo)
    //   }
    // },
    Capitalizable ( interesSimple, periodo ){
      let resultado = 0
      return resultado = Math.pow( (1 + ((interesSimple / 100 ) / periodo)), periodo ) - 1
    }
  }
}
</script>

<style lang="sass" scoped>
</style>
