<script setup>
import { onMounted, ref, computed, watch } from 'vue';
import Input from '../components/ui/Input.vue';
import Timer from '../components/ui/Timer.vue';
import StudySummaryModal from '../layouts/StudySummaryModal.vue';
import ComboBox from '../components/ui/ComboBox.vue';
import EditModal from '../components/EditModal.vue';

import { useUserStore } from "../stores/useUserStore";
import { useStudyStore } from "../stores/useStudyStore";
import { useSubjectStore } from "../stores/useSubjectStore";

import StudyCard from '../layouts/StudyCard.vue';


const studyStore = useStudyStore();
const userStore = useUserStore();
const subjectStore = useSubjectStore();

const selectedSubject = ref(null);
const chartData = ref();
const chartOptions = ref(null);
const isOpen = ref(false);

const isLoading = ref(true);

// Carregar as matérias da API
onMounted(async () => {
  try {
    await Promise.all([
      userStore.fetchUserSubjects(),
      subjectStore.fetchSubjects(),
      userStore.fetchUserStudyRecords(),
    ]);
    isLoading.value = false;
  } catch (error) {
    console.error("Error loading data: ", error);
  }
});

// Trecho responsavel pelo StudySummaryModal
const handleTimerStopped = () => {
  isOpen.value = true;
  updateChartData();
};

const handleCloseModal = () => {
  isOpen.value = false;
}

// Gera os dados do gráfico para cada registro
const getChartData = (record) => {
  const correctPercentage = userStore.getCorrectAnswerPercentage(record);
  const incorrectPercentage = userStore.getIncorrectAnswerPercentage(record);

  return {
    labels: ["Acertos", "Erros"],
    datasets: [
      {
        data: [correctPercentage, incorrectPercentage],
        backgroundColor: ["#00C49F", "#FF6384"],
        hoverBackgroundColor: ["#00B884", "#FF5675"],
      },
    ],
  };
};
// Gera as opções do gráfico para cada registro
const getChartOptions = (record) => {
  const correctPercentage = userStore.getCorrectAnswerPercentage(record);

  return {
    plugins: {
      legend: {
        display: false, // Oculta a legenda
      },
      tooltip: {
        enabled: false, // Desabilita o tooltip padrão
      },
      centerText: {
        text: `${correctPercentage.toFixed(1)}%`, // Texto inicial com a porcentagem de acertos
      },
    },
    rotation: -90,
    circumference: 180,
    cutout: "60%", // Espaço interno
  };
};

const updateChartData = () => {
  try {
    if (!userStore.userStudyRecords || userStore.userStudyRecords.length === 0) return;

    chartData.value = userStore.userStudyRecords.map(record => getChartData(record));
    chartOptions.value = userStore.userStudyRecords.map(record => getChartOptions(record));
  } catch (error) {
    console.error("Error updating chart data: ", error);
  }
};

// Atualizar dados sempre que necessário
watch(
  [() => userStore.userStudyRecords, () => subjectStore.subjects],
  ([newStudyRecords, newSubjects]) => {
    if (newStudyRecords && newSubjects) {
      updateChartData();
    }
  },
  { immediate: true }
);

// Combine userSubjects com nomes de matérias
const userSubjects = computed(() => {
  return userStore.userSubjects
    .map((subjectId) =>
      subjectStore.subjects.find((subject) => subject.id === subjectId)
    )
    .filter(Boolean); // Filtra valores nulos ou indefinidos
});
// Define a matéria selecionada na store
const handleSubjectSelection = (subject) => {
  selectedSubject.value = subject;
  studyStore.setSubject(subject.id); // Apenas atualiza o ID na store
};

const isSubjectSelected = computed(() => !!selectedSubject.value);

const isModalVisible = ref(false);
const selectedRecord = ref(null);

const openModal = (record) => {
  selectedRecord.value = { ...record };
  isModalVisible.value = true;
};

const updateRecord = (updatedRecord) => {
  console.log(updatedRecord); // Atualize o item no store ou na API
  isModalVisible.value = false;
};

</script>

<template>
  <div class="flex flex-col gap-4">
    <h3 class="text-4xl">Iniciar Estudos</h3>
    <div class="grid gap-2 grid-cols-6">
      <!-- Campo de pesquisa com lista suspensa de matérias -->
      <div class="flex gap-2 col-span-6">
        <ComboBox :options="userSubjects" :placeholder="'Selecione uma matéria...'" v-model="selectedSubject"
          @select="handleSubjectSelection" class="w-full" />
        <Input placeholder="Qual tópico você vai estudar?" :showLabel="false" class="w-full"
          v-model="studyStore.topic" />
      </div>
      <div class="gap-2 xl:col-span-2 lg:col-span-2 md:col-span-5 sm:col-span-5">
        <Timer :isDisabled="!isSubjectSelected" @timerStopped="handleTimerStopped" class="w-full" />
        <StudySummaryModal :isOpen="isOpen" @onClose="handleCloseModal" />
      </div>
      <div class="xl:col-span-4">
        <!-- Exibe os registros de estudo -->
        <div  class="grid gap-2 xl:grid-cols-2">
          <StudyCard v-for="(record, index) in userStore.userStudyRecords" :key="record.id" :record="record" :isLoading="isLoading"
            :chartData="chartData[index]" :chartOptions="chartOptions[index]" @edit="openModal" />
          <EditModal v-if="isModalVisible" :isVisible="isModalVisible" :record="selectedRecord" @update="updateRecord"
            @close="isModalVisible = false" />
        </div>
      </div>
    </div>
  </div>
</template>