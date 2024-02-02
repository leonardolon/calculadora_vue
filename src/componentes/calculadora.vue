<script setup>
    const props = defineProps (['estadoErrorVerdadeiro', 'estadoAutoCalcularVerdadeiro', 'estadoErrorMessage', 'estadoAutoCalcularFalso', 'estadoErrorFalso', 'estadoNumeroUm', 'estadoOperacaoEscolhidaSimbolo', 'EstadoEscolhaOperacaoNaoRaiz', 'estadoNumeroDois', 'resultadoCalculo'])
</script>

<template>
    <div class="calculadora">
        <h4 id="error-warning" :class="{ show : props.estadoErrorVerdadeiro && props.estadoAutoCalcularVerdadeiro }">{{ props.estadoErrorMessage }}</h4>
        <p v-show="props.estadoAutoCalcularFalso">Preencha os campos e clique em "Iniciar" para calcular!</p>
        <div class="calculo" :class="{ calculo__visivel : props.estadoAutoCalcularVerdadeiro && !props.estadoErrorFalso }">
            <p>{{ props.estadoNumeroUm }}</p>
            <p>{{ props.estadoOperacaoEscolhidaSimbolo }}</p>
            <p :class="{ invisivel: props.EstadoEscolhaOperacaoNaoRaiz}">{{ props.estadoNumeroDois }}</p>
            <p>=</p>
            <p>{{ props.resultadoCalculo }}</p>
        </div>
    </div>
</template>

<style lang="scss" scoped>

.calculadora {
    text-align: center;
    height: 100vh;

    #error-warning {
        margin-top: 14px;
        color: red;
        margin-bottom: 0;
        visibility: hidden;
        font-size: 20px;
        text-align: center;
    }

    #error-warning.show {
        color: red;
        visibility: visible;
    }

    .calculo {
        display: flex;
        justify-content: center;
        gap: 20px;
        visibility: hidden;
        padding-bottom: 10px;

        p {
            font-size: 50px;
            margin-top: 40px;
            margin-bottom: 150px;
        }

        &__visivel {
            visibility: visible;
            padding-bottom: 110px;

            p.invisivel {
                display: none;
            }
        }
    }

    p {
        font-size: 40px;
        margin-top: 40px;
        margin-bottom: 0;
    }
}

</style>