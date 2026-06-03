<script setup lang="ts">
import { ref } from 'vue';

interface Producto {
  id: number;
  nombre: string;
  precio: number;
  stock: number;
}

const isAuthenticated = ref<boolean>(false);
const username = ref<string>('');
const password = ref<string>('');
const errorMessage = ref<string>('');

const listaProductos: Producto[] = [
  { id: 101, nombre: "Teclado Mecánico RGB", precio: 250, stock: 12 },
  { id: 102, nombre: "Mouse Óptico Inalámbrico", precio: 110, stock: 20 },
  { id: 103, nombre: "Monitor Gamer 24' 144Hz", precio: 1450, stock: 5 }
];

const handleLogin = (): void => {
  if (username.value === 'felix.maldonado' && password.value === 'itpm2026') {
    isAuthenticated.value = true;
    errorMessage.value = '';
  } else {
    errorMessage.value = '❌ Credenciales incorrectas. Intente de nuevo.';
  }
};

const handleLogout = (): void => {
  isAuthenticated.value = false;
  username.value = '';
  password.value = '';
};
</script>

<template>
  <div class="container mt-5" style="font-family: sans-serif; max-width: 600px;">
    
    <!-- LOGIN FORM -->
    <div v-if="!isAuthenticated" class="card shadow border-0">
      <div class="card-header bg-dark text-white text-center py-3">
        <h4 class="mb-0">🔑 Sistema de Gestión - ITPM</h4>
        <small class="text-muted">Asignatura: DPW-207 | Estudiante: KEVIN CHOQUE APAZA</small>
      </div>
      <div class="card-body p-4">
        <form @submit.prevent="handleLogin">
          <div class="mb-3">
            <label class="form-label fw-bold">Usuario Docente</label>
            <input v-model="username" type="text" class="form-control" placeholder="Ej: felix.maldonado" required />
          </div>
          <div class="mb-3">
            <label class="form-label fw-bold">Contraseña</label>
            <input v-model="password" type="password" class="form-control" placeholder="........" required />
          </div>
          
          <div v-if="errorMessage" class="alert alert-danger py-2 text-center small mb-3">
            {{ errorMessage }}
          </div>

          <button type="submit" class="btn btn-primary w-100 fw-bold py-2 shadow-sm">
            Ingresar al Sistema
          </button>
        </form>
      </div>
    </div>

    <!-- ALMACEN PANEL -->
    <div v-else class="card shadow border-0">
      <div class="card-header bg-success text-white d-flex justify-content-between align-items-center py-3">
        <h5 class="mb-0">🚀 Panel de Almacén - Activo</h5>
        <button @click="handleLogout" class="btn btn-sm btn-light fw-bold shadow-sm">Salir</button>
      </div>
      <div class="card-body p-4 text-center">
        <h4 class="text-success fw-bold">¡Bienvenido, Lic. Félix Maldonado!</h4>
        <p class="text-muted small">Control de Inventarios en Tiempo Real</p>
        <hr />
        
        <ul class="list-group list-group-flush text-start">
          <li v-for="p in listaProductos" :key="p.id" class="list-group-item d-flex justify-content-between align-items-center py-3">
            <div>
              📦 <span class="fw-bold">{{ p.nombre }}</span>
              <br />
              <small class="text-muted">ID: {{ p.id }}</small>
            </div>
            <span class="badge bg-dark rounded-pill">Bs. {{ p.precio }} | Stock: {{ p.stock }} pzas.</span>
          </li>
        </ul>
      </div>
    </div>

  </div>
</template>