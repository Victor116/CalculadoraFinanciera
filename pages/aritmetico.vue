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
            
        //- AG
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey AG
            .card-content
              b-input(
                placeholder="AG"
                type="number"
                min="0"
                v-model="AG"
              )

        //- PT
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey PT
            .card-content
              b-input(
                placeholder="PT"
                type="number"
                min="0"
                v-model="PT"
              )

        //- AT
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey AT
            .card-content
              b-input(
                placeholder="AT"
                type="number"
                min="0"
                v-model="AT"
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

    p(style="margin-top: 2em") ¿Calcular PA? 
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

    p ¿Calcular AG? 
      b-switch(
        type="is-success"
        v-model="switchAG"
        true-value="Si"
        false-value="No"
        :click="CalcularAG()"
      ) {{ switchAG }}

    p ¿Calcular PT? 
      b-switch(
        type="is-success"
        v-model="switchPT"
        true-value="Si"
        false-value="No"
        :click="CalcularPT()"
      ) {{ switchPT }}

    p ¿Sumar o Restar PT? 
      b-switch(
        type="is-success"
        v-model="switchSumarPT"
        true-value="Sumar"
        false-value="Restar"
      ) {{ switchSumarPT }}

    p ¿Calcular AT? 
      b-switch(
        type="is-success"
        v-model="switchAT"
        true-value="Si"
        false-value="No"
        :click="CalcularAT()"
      ) {{ switchAT }}

    p ¿Sumar o Restar AT? 
      b-switch(
        type="is-success"
        v-model="switchSumarAT"
        true-value="Sumar"
        false-value="Restar"
      ) {{ switchSumarAT }}
</template>

<script>
export default {
  name: 'Calculadora',
  data: () => ({
    AA: 0, // Valor presente
    AG: 0,
    G:  0, // Gradiente
    PA: 0, 
    PG: 0, // p
    PT: 0,
    AT: 0,
    interes: 0, // i
    tiempo: 0, // n

    switchPA: 'No',
    switchPG: 'No',
    switchPT: 'No',
    switchAT: 'No',
    switchAG: 'No',
    switchSumarPT: 'Restar',
    switchSumarAT: 'Restar',

    radio: 0, // m
    switchCapitalizable: 'No',
  }),
  methods: {
    CalcularMathPow(interes, tiempo){
      return Math.pow((1 + interes), tiempo) 
    },
    CalcularPA  () {
      if(this.tiempo > 0 && this.interes > 0 && this.AA > 0 && this.switchPA == 'Si' && this.switchPG == 'No' && this.switchPT == 'No' && this.switchAG == 'No' ){
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
      if( this.G > 0 && this.interes > 0 && this.tiempo > 0 && this.switchPA == 'No' && this.switchPG == 'Si' && this.switchPT == 'No' && this.switchAG == 'No'){
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
      }
    },
    CalcularAG () {
      if( this.G > 0 && this.interes > 0 && this.tiempo > 0 && this.switchAG == 'Si' && this.switchPA == 'No' && this.switchPG == 'No' && this.switchPT == 'No' ){
        let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
          interesNeto = this.Capitalizable(this.interes, this.radio)

        let left = 1 / interesNeto
        let abajo1 = this.CalcularMathPow(interesNeto, this.tiempo) - 1
        let right = this.tiempo / abajo1
        this.AG = this.G * (left - right)
        console.log(this.AG)
      }
    },
    CalcularPT () {
      if( this.PA > 0 && this.PG > 0 && this.G > 0 && this.interes > 0 && this.tiempo > 0 && this.switchPT == 'Si' && this.switchAG == 'No' && this.switchPA == 'No' && this.switchPG == 'No' ){
        if(this.switchSumarPT == 'Restar')
          this.PT = this.PA - this.PG 
        else 
          this.PT = this.PA + this.PG   
      }
    },
    CalcularAT () {
      if( this.AA > 0 && this.AG > 0 && this.G > 0 && this.interes > 0 && this.tiempo > 0 && this.switchAT == 'Si' && this.switchAG == 'No' && this.switchPA == 'No' && this.switchPG == 'No' ){
        if(this.switchSumarAT == 'Restar')
          this.AT = this.AA - this.AG 
        else 
          this.AT = this.AA + this.AG   
      }
    },
    Capitalizable ( interesSimple, periodo ){
      let resultado = 0
      return resultado = Math.pow( (1 + ((interesSimple / 100 ) / periodo)), periodo ) - 1
    }
  }
}
</script>

<style lang="sass" scoped>
</style>
