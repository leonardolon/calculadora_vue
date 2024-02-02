<script setup>
import { reactive } from 'vue';
import Formulario from './components/Formulario.vue';
import Calculadora from './components/Calculadora.vue';

const estado = reactive ({
  autoCalcular: false,
  error: false,
  errormessage : "É necessário selecionar os valores para realizar o cálculo!",
  numero1 : "",
  numero2 : "",
  escolhaOperacao: "escolha",
  operacaoEscolhida: {},
  operacoes: [
    {
      titulo: 'soma',
      simbolo: '+',
      expressao: "parseFloat(estado.numero1) + parseFloat(estado.numero2)"
    },
    {
      titulo: 'subtracao',
      simbolo: '-',
      expressao: "parseFloat(estado.numero1) - parseFloat(estado.numero2)"
    },
    {
      titulo: 'multiplicacao',
      simbolo: 'x',
      expressao: "parseFloat(estado.numero1) * parseFloat(estado.numero2)"
    },
    {
      titulo: 'divisao',
      simbolo: '/',
      expressao: "parseFloat(estado.numero1) / parseFloat(estado.numero2)"
    },
    {
      titulo: 'potenciacao',
      simbolo: 'xⁿ',
      expressao: "parseFloat(estado.numero1) ** parseFloat(estado.numero2)"
    },
    {
      titulo: 'radiciacao',
      simbolo: '√',
      expressao: "Math.sqrt(parseFloat(estado.numero1))"
    }
  ]
})

const iniciarCalculo = () => {
  estado.autoCalcular = true;
  verificarErro();
}

const selecionarOperacao = (evento) => {
  estado.escolhaOperacao = evento.target.value;
  if (estado.escolhaOperacao !== "escolha") {
    const arrayEscolhido = estado.operacoes.filter(oper => oper.titulo == estado.escolhaOperacao);
    estado.operacaoEscolhida = arrayEscolhido[0];
  }
  if (estado.escolhaOperacao == "radiciacao") {
    estado.numero2 = "";
  }
  verificarErro();
}

const editarNumero = (evento) => {
  evento.target.id == "1"? estado.numero1 = evento.target.value : estado.numero2 = evento.target.value;
  if (isNaN(estado.numero2)) estado.numero2 = 0;
  verificarErro();
}

const getNumeroDois = () => (estado.escolhaOperacao == "radiciacao") ? estado.numero2 : parseFloat(estado.numero2).toLocaleString('pt-br', {maximumFractionDigits: 2});

const verificarErro = () => {
  (estado.numero1 === "" || estado.numero2 === "") || (estado.escolhaOperacao == "escolha") ? estado.error = true : estado.error = false;
  if ((estado.numero1.match(/[^$,.\d]/)) || (estado.numero2.match(/[^$,.\d]/))) estado.error = true;
  if ((estado.escolhaOperacao == "radiciacao") && (estado.numero1 !== "") && (estado.numero2 === "")) estado.error = false
  
  if ((estado.numero1 === "") || (estado.numero2 === "")) estado.errormessage = "É necessário inserir valores válidos para realizar o cálculo!";
  if (estado.escolhaOperacao == "radiciacao") estado.errormessage = "É necessário inserir um valor válido para calcular sua raiz quadrada!";
  if (estado.escolhaOperacao == "escolha") estado.errormessage = "É necessário selecionar um tipo de operação para realizar o cálculo!";
}

const somenteNumeros = (evento) => {
  const keysAllowed = ['0','1','2','3','4','5','6','7','8','9', '.', ',','Enter'];
  const keyPressed = evento.key;
  if (!keysAllowed.includes(keyPressed)) {
    evento.preventDefault();
  }
  if (keyPressed == '.') {
    evento.preventDefault();
    evento.target.value += ',';
  }
}

const resultadoCalculo = () => ((eval(estado.operacaoEscolhida.expressao)*100)/100).toLocaleString('pt-br', {maximumFractionDigits: 2});

</script>



<template>
  <div class="container">
    <header class="header">
      <h1>Calculadora VueJS</h1>
    </header>
    <Formulario :iniciar-calculo="iniciarCalculo"
                :editar-numero="editarNumero"
                :verifica-escolha-nao-raiz="estado.escolhaOperacao !== 'radiciacao'"
                :valor-numero-dois="estado.numero2"
                :valor-numero-um="estado.numero1"
                :selecionar-operacao="selecionarOperacao"
                :estado-auto-calcular-falso="!estado.autoCalcular"
                :somente-numeros="somenteNumeros"
                :estado-auto-calcular-verdadeiro="estado.autoCalcular" />
    <Calculadora  :estado-error-verdadeiro="estado.error"
                  :estado-auto-calcular-verdadeiro="estado.autoCalcular"
                  :estado-error-message="estado.errormessage"
                  :estado-auto-calcular-falso="!estado.autoCalcular"
                  :estado-error-falso="estado.error"
                  :estado-numero-um="parseFloat(estado.numero1).toLocaleString('pt-br', {maximumFractionDigits: 2})"
                  :estado-operacao-escolhida-simbolo="estado.operacaoEscolhida.simbolo"
                  :estado-escolha-operacao-nao-raiz="estado.escolhaOperacao !== 'radiciacao'"
                  :estado-numero-dois="getNumeroDois()"
                  :resultado-calculo="resultadoCalculo()" />  
  </div>
</template>

<style lang="scss" scoped>

body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Roboto', sans-serif;
}

.container {
  background-color: rgb(189, 249, 111);
}

.header {
  background-color: #E7E7E7;
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  justify-content: center;

  h1 {
    font-size: 40px;
    font-weight: bold;
    padding-top: 5px;
    padding-bottom: 5px;
    margin-top: 30px;
    margin-bottom: 10px;
  }
}

</style>