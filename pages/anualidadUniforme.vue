<template lang="pug">
  .notification
    h1.subtitle Anuaidad Uniforme
      span.icon(style="margin-left: 1em")
        i.fas.fa-chart-bar

    .column.is-12-desktop.is-12-mobile
      .columns.is-multiline.is-mobile
        //- A
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey A
            .card-content
              b-input(
                placeholder="A"
                type="number"
                min="0"
                v-model="A"
              )

        //- P
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey P
            .card-content
              b-input(
                placeholder="P"
                type="number"
                min="0"
                v-model="P"
              ) 

        //- F
        .column.is-3-desktop.is-6-mobile
          .card
            p.card-header-title.has-text-grey F
            .card-content
              b-input(
                placeholder="F"
                type="number"
                min="0"
                v-model="F"
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

    p(style="margin-top: 2em") ¿Calcular P? 
      b-switch(
        type="is-success"
        v-model="switchP"
        true-value="Si"
        false-value="No"
        :click="CalcularP()"
      ) {{ switchP }}

    p ¿Calcular F? 
      b-switch(
        type="is-success"
        v-model="switchF"
        true-value="Si"
        false-value="No"
        :click="CalcularF()"
      ) {{ switchF }}

    p ¿Calcular A? 
      b-switch(
        type="is-success"
        v-model="switchA"
        true-value="Si"
        false-value="No"
        :click="CalcularA()"
      ) {{ switchA }}

</template>

<script>
export default {
  name: 'Calculadora',
  data: () => ({
    A: 0, 
    P: 0, // Valor presente
    F:  0, // futuro
    interes: 0, // i
    tiempo: 0, // n

    switchA: 'No',
    switchP: 'No',
    switchF: 'No',

    radio: 0, // m
    switchCapitalizable: 'No',
  }),
  methods: {
    CalcularMathPow(interes, tiempo){
      return Math.pow((1 + interes), tiempo) 
		},
    CalcularP(){
			if(this.A > 0 && this.interes > 0 && this.tiempo > 0 && this.switchA == 'No' && this.switchP == 'Si' && this.switchF == 'No'){ 
				let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
          interesNeto = this.Capitalizable(this.interes, this.radio)

				let arriba = this.CalcularMathPow(interesNeto, this.tiempo) - 1
				let abajo = interesNeto * this.CalcularMathPow(interesNeto, this.tiempo) 
				this.P = this.A * (arriba / abajo)
			}
		},
		CalcularF(){
			if(this.A > 0 && this.interes > 0 && this.tiempo > 0 && this.switchA == 'No' && this.switchP == 'No' && this.switchF == 'Si'){
				let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
					interesNeto = this.Capitalizable(this.interes, this.radio)
					
				let arriba = this.CalcularMathPow(interesNeto, this.tiempo) - 1
				let adentro = arriba / interesNeto
				this.F = this.A * adentro
			}
		},
		CalcularA(){
			if(this.interes > 0 && this.tiempo > 0 && this.switchA == 'Si' && this.switchP == 'No' && this.switchF == 'No'){
				let interesNeto = 0

        if(this.switchCapitalizable == 'No')
          interesNeto = this.interes / 100
        else
					interesNeto = this.Capitalizable(this.interes, this.radio)

				if(this.P > 0 && this.F == 0){
					let arriba = interesNeto * this.CalcularMathPow(interesNeto, this.tiempo)
					let abajo = this.CalcularMathPow(interesNeto, this.tiempo) - 1
					let adentro = arriba / abajo
					this.A = this.P * adentro
				} else {
					let arriba = interesNeto
					let abajo = this.CalcularMathPow(interesNeto, this.tiempo) - 1
					let adentro = arriba / abajo
					this.A = this.F * adentro
				}
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
