<template>
  <div class="column">
    <div class="tile is-child notification is-primary">
      <form @submit.prevent>

        <!-- elemento -->
        <div class="field is-horizontal" v-for='(categoria, i) in categorias' :key='i'>
          <div class="field-label is-normal">
            <label class="label">{{ categoria }}</label>
          </div>
          <div class="field-body">
            <div class="field">
              <p class="control">
                <input :class="{'input': true, 'is-danger': regexErro}" type="text" ref="input" :disabled='indisponivel' @blur="tratarResposta(i)">
              </p>
            </div>
          </div>
        </div>
        <!-- elemento -->

        <button class="button is-block is-primary is-large is-fullwidth is-inverted" @click="arregar()">arregar</button>
      </form>
    </div>
  </div>

</template>

<script>
export default {
  props: ['categorias','indisponivel','letra','jsonRespostas'],
  data () {
    return {
      classe: 'is-danger',
      regexErro: false
    }
  },
  methods: {
    arregar(){
      this.$emit('arregar');
    },
    responde(){
      this.$emit('responde');
    },
    tratarResposta(i) {
      // /\b[t]+/gi
      var regex = new RegExp('\\b['+this.letra+']+', 'gi');

      if(regex.test(this.$refs.input[i].value)) {
        // escolhe cor da margem
        this.$refs.input[i].className = 'input is-success';

        // Formata string
        var resposta = this.$refs.input[i].value.charAt(0).toUpperCase() + this.$refs.input[i].value.slice(1).toLowerCase();

        // att input co string formatada
        this.$refs.input[i].value = resposta;

        // att resposta que vai ao json
        this.jsonRespostas[i] = resposta;

        this.responde();
      } else {
        // escolhe cor da margem
        this.$refs.input[i].className = 'input is-danger';

        // apaga o campo automaticamente
        this.$refs.input[i].value = null;
      }
    }
  }
}
</script>
<style scoped>
.is-danger, .is-success{
  border-width: 4px !important;
}
</style>
