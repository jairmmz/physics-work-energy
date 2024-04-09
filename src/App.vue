<template>
  <div class="mt-5 flex flex-col items-center justify-center">
    <h1 class="text-xl font-bold">Trabajo y Energía</h1>
    <div class="w-[80%] mb-4">
      <div class="mb-4">
        <span class="">
        Una partícula de masa 2[kg]posee una rapidez de12[m s] al pasar por la posición A mostrada en la figura. Si en
        el tramo BC se disipa un 20% de la energía cinética inicial por efecto del roce, determine la rapidez de la
        partícula al pasar por la posición D. En los tarmos AB y CD no hay roce.
        </span>
      </div>

      <!-- Crear dos columnas para el form y la imagen -->
      <div class="flex justify-center border p-2 rounded">
        <div class="w-[55%]">
          <form @submit.prevent="calculateVelocity">
            <div class="mb-4">
              <span>Masa de la partícula (kg): </span>
              <input type="text" v-model="form.particleMass"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" required>
            </div>
            <div class="mb-4">
              <span>Rapidez en A (m/s): </span>
              <input type="text" v-model="form.velocityA"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" required>
            </div>
            <div class="mb-4">
              <span>Porcentaje de disipación en BC (%): </span>
              <input type="text" v-model="form.dissipation"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" required>
            </div>
            <div class="mb-4">
              <span>Altura de D (m): </span>
              <input type="text" v-model="form.heightD"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" required>
            </div>
            <button type="submit"
              class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">Calcular</button>
          </form>
        </div>
        <div class="w-[70%]">
          <img src="/src/assets/image_work_energy.png" alt="">
        </div>
      </div>

      <!-- Resultado -->
      <div v-show="isResultVisible" class="mt-4">
        <span>Energía cinética en A: <strong>{{ resultEnergyA }}</strong></span> <br>
        <span>Energía cinética en BC: <strong>{{ resultEnergyBC }}</strong></span> <br>
        <span>Energía cinética en D: <strong>{{ resultEnergyC }}</strong></span> <br>
        <span>Rapidez en D: <strong>{{ resultVelocityD }}</strong></span>
      </div>

      <div>
        <!-- Katex -->
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const resultVelocityD = ref<string>('');
const resultEnergyA = ref<string>('');
const resultEnergyBC = ref<string>('');
const resultEnergyC = ref<string>('');

const isResultVisible = ref<boolean>(false);


interface IForm {
  particleMass: number | string;
  velocityA: number | string;
  dissipation: number | string;
  heightD: number | string;
}

// Crear formulario para los datos
const form = ref<IForm>({
  particleMass: '',
  velocityA: '',
  dissipation: '',
  heightD: ''
});

// Calcular la rapidez de la partícula en D
const calculateVelocity = () => {
  // Calcular la energía cinética inicial en el punto A.
  const kineticEnergyA = 0.5 * Number(form.value.particleMass) * Math.pow(Number(form.value.velocityA), 2);
  resultEnergyA.value = ` ${kineticEnergyA.toFixed(2)} J.`;

  // Energía cinética en B
  const kineticEnergyB = kineticEnergyA;

  // Calculando la fricción en el tramo BC
  const frictionBC = kineticEnergyB * (Number(form.value.dissipation) / 100);
  resultEnergyBC.value = ` ${frictionBC.toFixed(2)} J.`;

  // Calculando la energía mecánica en C
  const mechanicalEnergyC = kineticEnergyB - frictionBC;
  resultEnergyC.value = ` ${mechanicalEnergyC.toFixed(2)} J.`;

  const valueGravity = 10;

  // Hallando la velocidad en C
  const velocityC = Math.sqrt(2 * Number(mechanicalEnergyC) / Number(form.value.particleMass) - 2 * valueGravity * Number(form.value.heightD));


  // Mostrar el resultado
  resultVelocityD.value = ` ${velocityC.toFixed(2)} m/s.`;

  isResultVisible.value = true;
};

</script>


<style scoped></style>
