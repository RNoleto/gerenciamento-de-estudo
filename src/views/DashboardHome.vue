<script setup>
import StudyTimeRecord from '../layouts/StudyTimeRecord.vue';
import StudyStatisticsRecords from '../layouts/StudyStatisticsRecords.vue';
import SubjectSummaryTable from '../layouts/SubjectSummaryTable.vue';

import { useUserStore } from '../stores/useUserStore';
const userStore = useUserStore();

</script>

<template>
    <div class="flex flex-col gap-4 p-4">
        <div class="flex flex-col gap-2">
            <h3 class="text-4xl font-bold">Resumo dos estudos</h3>
            <p>Carreira: {{ userStore.careerName }}</p>
        </div>
        <div class="flex flex-wrap gap-2">
            <StudyTimeRecord/>
            <StudyStatisticsRecords />
            <SubjectSummaryTable />
        </div>
    </div>
</template>