<script setup>
    const props = defineProps (['iniciarCalculo', 'editarNumero', 'verificaEscolhaNaoRaiz', 'valorNumeroDois', 'valorNumeroUm', 'selecionarOperacao', 'estadoAutoCalcularFalso', 'estadoAutoCalcularVerdadeiro', 'somenteNumeros'])
</script>

<template>
    <form class = "form" id="formulario-inputs" @submit.prevent="evento => props.iniciarCalculo()" novalidate>
        <input class="input" inputmode="numeric" id="1" type="number" placeholder="Número 1" @keypress="evento => props.somenteNumeros(evento)" @change="evento => props.editarNumero(evento)" :value="props.valorNumeroUm"/>
        <input class="input" inputmode="numeric" id="2" type="number" placeholder="Número 2" @keypress="evento => props.somenteNumeros(evento)" @change="evento => props.editarNumero(evento)" :value="props.valorNumeroDois" v-show="props.verificaEscolhaNaoRaiz"/>
        <div class="operacao">
            <select class="select" @change="evento => props.selecionarOperacao(evento)">
                <option class="item" selected disabled value="escolha">Escolha a Operação:</option>
                <option class="item" value="soma">Soma</option>
                <option class="item" value="subtracao">Subtração</option>
                <option class="item" value="multiplicacao">Multiplicação</option>
                <option class="item" value="divisao">Divisão</option>
                <option class="item" value="potenciacao">Potenciação</option>
                <option class="item" value="radiciacao">Raiz Quadrada</option>
            </select>
        </div>
        <button v-show="props.estadoAutoCalcularFalso" id="botao-calcular" type="submit">Iniciar</button>
    </form>
</template>

<style lang="scss" scoped>
.form{
    display: flex;
    align-items: center;
    gap: 20px;
    width: 100%;
    justify-content: center;
    margin-top: 30px;

    input {
        font-size: 18px;
        background-color: #fff;
        border: none;
        border-bottom: 2px solid #000;
        padding: 8px;
        border-radius: 5px;
        width: 110px;
    }

    #botao-calcular {
        background-color: #009432;
        color: #fff;
        font-size: 24px;
        font-weight: bold;
        border: none;
        cursor: pointer;
        padding: 8px;
        border-radius: 10px;
    }

    #botao-calcular:hover {
        background-color: #00802b;
    }

    #botao-calcular:disabled {
        cursor: default;
        background-color: darken(#00802b, 10%);
        color: #888787;
    }

    .select {
        font-size: 18px;
        background-color: #fff;
        border: none;
        border-bottom: 2px solid #000;
        padding: 8px;
        border-radius: 5px;
        width: 210px;
        text-align: center;

        .item {
            font-size: 15px;
        }
    }
}

@media (max-width: 620px) {
    form {
        flex-direction: column;
    }

    #botao-calcular {
        width: 80%;
        margin-left: 30px;
    }
}
</style>