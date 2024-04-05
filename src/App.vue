<template>
  <div class="mt-5 flex flex-col items-center justify-center">
    <div class="w-[80%] mb-4">
      <div class="mb-4">
        <span class="">
        Una partícula de masa 2[kg]posee una rapidez de12[m s] al pasar por la posición A mostrada en la figura. Si en
        el
        tramo BC se disipa un 20% de la energía cinética inicial por efecto del roce, determine la rapidez de la
        partícula
        al pasar por la posición D. En los tarmos AB y CD no hay roce.
        </span>
      </div>
      <!-- Crear dos columnas para el form y la imagen -->
      <div class="flex justify-center">
        <div class="w-[55%]">
          <form>
            <div class="mb-4">
              <span>Masa de la partícula (kg): </span>
              <input type="text" v-model="form.particleMass"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
            </div>
            <div class="mb-4">
              <span>Rapidez en A (m/s): </span>
              <input type="text" v-model="form.velocityA"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
            </div>
            <div class="mb-4">
              <span>Porcentaje de disipación en BC (%): </span>
              <input type="text" v-model="form.dissipation"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
            </div>
            <div class="mb-4">
              <span>Altura de D (m): </span>
              <input type="text" v-model="form.heightD"
                class="shadow appearance-none border rounded py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
            </div>
            <button type="submit" @click.prevent="calculateVelocity"
              class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">Calcular</button>
          </form>
          <!-- Resultado -->
          <div class="mt-4">
            <span>Resultado: {{ result }}</span>
          </div>
        </div>
        <div class="w-[70%]">
          <img src="/src/assets/image_work_energy.png" alt="">
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const result = ref<string>('');

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

  // Calcular la energía cinética al final del tramo BC.
  const kineticEnergyBC = kineticEnergyA * (1 - Number(form.value.dissipation) / 100);

  // Calcular la energía cinética final en el punto D.
  const gravity:number = 9.81;
  const kineticEnergyD = kineticEnergyBC + Number(form.value.particleMass) * gravity * Number(form.value.heightD);

  // Calcular la energía mecánica total en el punto D.
  const mechanicalEnergyD = kineticEnergyBC + kineticEnergyD;

  // Determinar la rapidez de la partícula en el punto D.
  const velocityD = Math.sqrt(2 * mechanicalEnergyD / Number(form.value.particleMass));

  // Mostrar el resultado
  result.value = `La rapidez de la partícula en el punto D es de ${velocityD.toFixed(2)} m/s.`;

  // Dado que no hay disipación en el tramo CD, la energía cinética en el punto D es la misma que en el punto B.
  console.log(form.value);
}

</script>


<style scoped></style>
