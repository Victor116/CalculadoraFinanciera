<template lang="pug">
  .notification
    h1.subtitle Gradiente Aritmetico
      span.icon(style="margin-left: 1em")
        i.fas.fa-chart-line

    .column.is-12-desktop.is-12-mobile
      .columns.is-multiline.is-mobile
        //- A1
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey A1
            .card-content
              b-input(
                placeholder="A1"
                type="number"
                min="0"
                v-model="A1"
              )
              
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

        //- g
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey g
            .card-content
              b-input(
                placeholder="g"
                type="number"
                min="0"
                v-model="g"
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

    p(style="margin-top: 2em") ¿Calcular Futuro? 
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

    p ¿Calcular Pg? 
      b-switch(
        type="is-success"
        v-model="switchPg"
        true-value="Si"
        false-value="No"
        :click="CalcularPg()"
      ) {{ switchPg }}

</template>

<script>
export default {
  name: 'Calculadora',
  data: () => ({
    A1: 0, // Primera inversion
    interes: 0, // i
    PG: 0,
    g: 0, // Gradiente
    tiempo: 0, // n
    futuro: 0,
    presente: 0,

    radio: 0,
    switchFuturo: 'No',
    switchPresente: 'No',
    switchPg: 'No',
    switchCapitalizable: 'No',
  }),
  methods: {
    CalcularMathPow(interes, tiempo){
      return Math.pow((1 + interes), tiempo) 
    },
    CalcularFuturo (){
      if(this.A1 > 0 && this.interes > 0 && this.tiempo > 0 && this.switchFuturo == 'Si' && this.switchPresente == 'No' && this.switchPg == 'No'){
        let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
          interesNeto = this.Capitalizable(this.interes, this.radio)

        this.futuro = this.presente * this.CalcularMathPow(interesNeto, this.tiempo)
      }
    },
    CalcularPresente (){
      if(this.futuro > 0 && this.interes > 0 && this.tiempo > 0 && this.switchFuturo == 'No' && this.switchPresente == 'Si' && this.switchPg == 'No'){
        let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
          interesNeto = this.Capitalizable(this.interes, this.radio)

        this.presente = this.futuro / (this.CalcularMathPow(interesNeto, this.tiempo))
      }
    },
    CalcularPg (){
      if(this.A1 > 0 && this.g > 0 && this.interes > 0 && this.tiempo > 0 && this.switchFuturo == 'No' && this.switchPresente == 'No' && this.switchPg == 'Si'){
        let interesNetoi = 0
        let interesNetog = 0

        if(this.switchCapitalizable == 'No'){
          interesNetoi = this.interes / 100
          interesNetog = this.g / 100
        } else {
          interesNetoi = this.Capitalizable(this.interes, this.radio)
          interesNetog = this.g / 100
        }
        
        if(this.interes == this.g)
          this.PG = this.A1 * (this.tiempo / (1 + interesNetoi))
        else {
          let arriba2 = Math.pow(((1 + interesNetog) / (1 + interesNetoi)), this.tiempo)
          let arriba1 = 1 - arriba2
          let abajo1 = interesNetoi - interesNetog
          this.PG = this.A1 * (arriba1 / abajo1)
        }
      }
    },
    Capitalizable ( interesSimple, tiempo ){
      let resultado = 0
      return resultado = Math.pow( (1 + ((interesSimple / 100 ) / tiempo)), tiempo ) - 1
    }
  }
}
</script>

<style lang="sass" scoped>
</style>
