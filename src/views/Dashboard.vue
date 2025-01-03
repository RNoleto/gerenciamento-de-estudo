<script setup>
import { RouterView, useRoute } from 'vue-router';
import { onMounted, ref } from 'vue';
import { useUserStore } from '../stores/useUserStore';

const route = useRoute();

const userStore = useUserStore();

// Estado para controlar se a sidebar está recolhida ou expandida
const isSidebarCollapsed = ref(false);

// Estados para controlar a abertura dos menus colapsáveis
const isMenu1Open = ref(false);
const isMenu2Open = ref(false);
const isMenu3Open = ref(false);

// Funções para alternar os estados dos menus
const toggleMenu1 = () => (isMenu1Open.value = !isMenu1Open.value);
const toggleMenu2 = () => (isMenu2Open.value = !isMenu2Open.value);
const toggleMenu3 = () => (isMenu3Open.value = !isMenu3Open.value);

const toggleSidebar = () => {
  isSidebarCollapsed.value = !isSidebarCollapsed.value;
}

onMounted(async () => {
  await userStore.checkUserCareer();
});
</script>

<template>
    <div class="flex bg-gray-100">
      <!-- Sidebar -->
      <aside :class="[
        'min-h-screen bg-white shadow-md transition-all duration-300',
        isSidebarCollapsed ? 'w-16' : 'w-64'
      ]">
        <div class="p-4 text-lg font-bold border-b flex justify-between items-center">
          <span v-if="!isSidebarCollapsed">Área do Aluno</span>
          <button @click="toggleSidebar" class="p-2 text-gray-700 hover:bg-gray-200 rounded">
            <i :class="isSidebarCollapsed ? 'fas fa-angle-right' : 'fas fa-angle-left'"></i>
          </button>
        </div>
        <nav :class="isSidebarCollapsed ? 'p-1' : 'p-2'">
          <!-- Home da Dashboard -->
          <div>
            <a
            href="/area-do-aluno"
            :class="[
              'flex items-center gap-2 px-4 py-2 mt-4 rounded',
              route.path === '/area-do-aluno' ? 'bg-blue-100 text-blue-700' : 'text-gray-700 hover:bg-gray-100'
            ]"
          >
            <i class="fas fa-home"></i>
            <span v-if="!isSidebarCollapsed">Home</span>
          </a>
          </div>
          <!-- Estudar da Dashboard -->
          <div>
            <a
            href="/area-do-aluno/estudar"
            :class="[
              'flex items-center gap-2 px-4 py-2 rounded',
              route.path === '/area-do-aluno/estudar' ? 'bg-blue-100 text-blue-700' : 'text-gray-700 hover:bg-gray-100'
            ]"
          >
            <i class="fa-solid fa-stopwatch"></i>
            <span v-if="!isSidebarCollapsed">Estudar</span>
          </a>
          </div>
          <!-- Menu Principal 1 -->
          <div>
            <button @click="toggleMenu1"
              class="flex gap-1 items-center justify-between w-full px-4 py-2 text-left text-gray-700 hover:bg-gray-100 rounded">
              <div class="flex items-center gap-2">
                <i class="fa-solid fa-brain"></i>
                <span v-if="!isSidebarCollapsed">Menu 1</span>
              </div>
              <i class="fas" :class="[isMenu1Open ? 'fa-chevron-up' : 'fa-chevron-down', isSidebarCollapsed ? 'text-sm' : 'text-base']"></i>
            </button>
            <div v-if="isMenu1Open" :class="['mt-2 space-y-2 transition-all duration-300', isSidebarCollapsed ? 'absolute left-16 bg-white shadow-lg w-48' : 'pl-8']">
              <a href="#" class="block text-sm text-gray-600 hover:text-gray-900">Submenu 1</a>
              <a href="#" class="block text-sm text-gray-600 hover:text-gray-900">Submenu 2</a>
            </div>
          </div>
  
          <!-- Menu Principal 2 -->
          <div class="mt-4">
            <button @click="toggleMenu2"
              class="flex gap-1 items-center justify-between w-full px-4 py-2 text-left text-gray-700 hover:bg-gray-100 rounded">
              <div class="flex items-center gap-2">
                <i class="fas fa-chart-bar"></i>
                <span v-if="!isSidebarCollapsed">Relatórios</span>
              </div>
              <i class="fas" :class="[isMenu2Open ? 'fa-chevron-up' : 'fa-chevron-down', isSidebarCollapsed ? 'text-sm' : 'text-base']"></i>
            </button>
            <div v-if="isMenu2Open" :class="['mt-2 space-y-2 transition-all duration-300', isSidebarCollapsed ? 'absolute left-16 bg-white shadow-lg w-48' : 'pl-8']">
              <a href="#" class="block text-sm text-gray-600 hover:text-gray-900">Submenu 1</a>
              <a href="#" class="block text-sm text-gray-600 hover:text-gray-900">Submenu 2</a>
            </div>
          </div>
  
          <!-- Menu de Configuração -->
          <div class="mt-4">
            <button @click="toggleMenu3"
              class="flex gap-1 items-center justify-between w-full px-4 py-2 text-left text-gray-700 hover:bg-gray-100 rounded">
              <div class="flex items-center gap-2">
                <i class="fas fa-cog"></i>
                <span v-if="!isSidebarCollapsed">Configurações</span>
              </div>
              <i class="fas" :class="[isMenu3Open ? 'fa-chevron-up' : 'fa-chevron-down', isSidebarCollapsed ? 'text-sm' : 'text-base']"></i>
            </button>
            <div v-if="isMenu3Open" :class="['mt-2 space-y-2 transition-all duration-300', isSidebarCollapsed ? 'absolute left-16 bg-white shadow-lg w-48' : 'pl-8']">
              <a href="/area-do-aluno/carreiras" class="block text-sm text-gray-600 hover:text-gray-900"><i class="fa-solid fa-user-astronaut"></i> Minha Carreira</a>
              <a href="/area-do-aluno/materias" class="block text-sm text-gray-600 hover:text-gray-900"><i class="fa-solid fa-book"></i> Minhas Matérias</a>
              <a href="/area-do-aluno/ciclo-de-estudos" class="block text-sm text-gray-600 hover:text-gray-900"><i class="fa-solid fa-arrows-spin"></i> Meu Ciclo de Estudo</a>
              <a href="#" class="block text-sm text-gray-600 hover:text-gray-900"><i class="fa-solid fa-crosshairs"></i> Meus Objetivos</a>
            </div>
          </div>
        </nav>
      </aside>
      <!-- Conteúdo Principal -->
      <main class="flex-1 p-6">
        <!-- Conteudo a ser carregado na página -->
        <router-view />
      </main>
    </div>
</template>
