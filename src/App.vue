<script setup>
    import { ref, computed, watch } from 'vue' // Las 2 formas que se declaran el state: ref, reactive
    import Header from './components/Header.vue'
    import Button from './components/Button.vue'
    import { calcularTotalPagar } from './helpers'

    const cantidad = ref(10000);
    const meses = ref(6);
    const total = ref( calcularTotalPagar(cantidad.value, meses.value) );

    const MIN = 0;
    const MAX = 20000;
    const STEP = 100;

    const formatearDinero = (valor) => {
        const formatter = new Intl.NumberFormat('en-US', {
            style: 'currency',
            currency: 'USD'
        });
        return formatter.format(valor)
    };

    const pago = computed(() => {
        return total.value / meses.value
    });

    watch([cantidad, meses], () => {
        total.value = calcularTotalPagar(cantidad.value, meses.value);
    });

    const handleChangeDecremento = () => {
        const valor = cantidad.value - STEP;

        if (valor < MIN) {
            alert('Cantidad no válida');
            return;
        }

        cantidad.value = cantidad.value - STEP;
    }

    function handleChangeIncremento() {
        const valor = cantidad.value + STEP;
        if (valor > MAX) {
            alert('Cantidad no válida');
            return;
        }
        cantidad.value = cantidad.value + STEP;
    }
</script>

<template>
    <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
        <Header />        

        <div class="flex justify-between my-6">
            <Button
                :operador="'-'"
                @fn="handleChangeDecremento"
            />
            <Button
                :operador="'+'"
                @fn="handleChangeIncremento"
            />
        </div>


        <input 
            type='range' 
            class="mt-2 w-full h-6 bg-gray-200 accent-lime-500 hover:accent-lime-600"
            :min="MIN"
            :max="MAX"
            :step="STEP"
            v-model.number="cantidad"
        />

        <p class="text-center my-10 text-5xl font-extrabold text-indigo-600">{{formatearDinero(cantidad)}}  </p>

        <h2 class='text-2xl font-extrabold text-gray-500 text-center'>
            Elige un <span class="text-indigo-600">Plazo  </span>a pagar
        </h2>

        <select
            class="mt-5 w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500"
            :value=meses
            v-model.number="meses"
        >
            <option value="6">6 meses</option>
            <option value="12">12 meses</option>
            <option value="24">24 meses</option>
        </select>

        <div class="my-5 space-y-3 bg-gray-50 p-5">
            <h2 class='text-2xl font-extrabold text-gray-500 text-center'>
                Resumen <span class="text-indigo-600">de pagos  </span>
            </h2>
            <p class='text-xl text-gray-500 text-center font-bold'>{{ meses }} Meses</p>
            <p class='text-xl text-gray-500 text-center font-bold'>{{ formatearDinero( total) }} Total a pagar</p>
            <p class='text-xl text-gray-500 text-center font-bold'>{{ formatearDinero( pago) }} Mensuales</p>
        </div>
    </div>
</template>
