<script setup>
import Career from '../components/ui/Career.vue';
import Button from '../components/ui/Button.vue';
import Search from '../components/ui/Search.vue';
</script>

<template>
    
        <h2 class="mb-4 font-bold text-xl">Selecione as matérias que deseja estudar.</h2>
        <Search placeholder="Pesquise a matéria..."/>
        <div class="mt-4 flex flex-wrap gap-2 w-full">
            <Career icon="basil:book-outline" careerName="Língua Portuguesa"/>
            <Career icon="basil:book-outline" careerName="Informática"/>
            <Career icon="basil:book-outline" careerName="Matemática"/>
            <Career icon="basil:book-outline" careerName="Direito Administrativo"/>
            <Career icon="basil:book-outline" careerName="Raciocínio Lógico"/>
            <Career icon="basil:book-outline" careerName="Direito Constitucional"/>
            <Career icon="basil:book-outline" careerName="Língua Inglesa"/>
            <Career icon="basil:book-outline" careerName="Direito Civil"/>
        </div>
        <Button class="float-right">Avançar</Button>
    
</template>