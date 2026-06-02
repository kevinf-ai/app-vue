<script setup lang="ts">
interface Producto {
  id: number
  nombre: string
  precio: number
  stock: number
}

const listaProductos: Producto[] = [
  { id: 101, nombre: 'Teclado Mecánico RGB', precio: 250, stock: 12 },
  { id: 102, nombre: 'Mouse Óptico Inalámbrico', precio: 110, stock: 20 },
]

const totalUnidades = listaProductos.reduce((acumulado, producto) => acumulado + producto.stock, 0)
const valorInventario = listaProductos.reduce(
  (acumulado, producto) => acumulado + producto.precio * producto.stock,
  0,
)

const formatoMoneda = new Intl.NumberFormat('es-BO', {
  style: 'currency',
  currency: 'BOB',
  maximumFractionDigits: 0,
})

const obtenerEstado = (stock: number) => {
  if (stock >= 15) return 'Disponible'
  if (stock >= 8) return 'Stock medio'
  return 'Bajo stock'
}
</script>

<template>
  <main class="warehouse-page">
    <section class="hero">
      <div class="hero__content">
        <p class="eyebrow">Panel de almacén</p>
        <h1>Control elegante de productos para ITPM</h1>
        <p class="hero__text">
          Vista rápida del inventario, cantidades disponibles y valor total del stock.
        </p>

        <div class="hero__stats">
          <article class="stat-card">
            <span class="stat-card__label">Productos</span>
            <strong>{{ listaProductos.length }}</strong>
          </article>
          <article class="stat-card">
            <span class="stat-card__label">Unidades</span>
            <strong>{{ totalUnidades }}</strong>
          </article>
          <article class="stat-card">
            <span class="stat-card__label">Valor inventario</span>
            <strong>{{ formatoMoneda.format(valorInventario) }}</strong>
          </article>
        </div>
      </div>

      <div class="hero__badge">
        <span>DPW-207</span>
        <strong>KEVIN CHOQUE APAZA</strong>
      </div>
    </section>

    <section class="inventory-grid">
      <article v-for="producto in listaProductos" :key="producto.id" class="product-card">
        <div class="product-card__top">
          <span class="product-id">#{{ producto.id }}</span>
          <span
            class="status-pill"
            :class="`status-pill--${obtenerEstado(producto.stock).toLowerCase().replace(' ', '-')}`"
          >
            {{ obtenerEstado(producto.stock) }}
          </span>
        </div>

        <h2>{{ producto.nombre }}</h2>

        <dl class="product-meta">
          <div>
            <dt>Precio</dt>
            <dd>{{ formatoMoneda.format(producto.precio) }}</dd>
          </div>
          <div>
            <dt>Stock</dt>
            <dd>{{ producto.stock }} pzas.</dd>
          </div>
        </dl>

        <div class="product-progress">
          <div class="product-progress__bar" :style="{ width: `${Math.min(producto.stock * 5, 100)}%` }"></div>
        </div>
      </article>
    </section>
  </main>
</template>

<style scoped>
.warehouse-page {
  min-height: 100vh;
  padding: 32px 20px 48px;
  color: #f8fafc;
  background:
    radial-gradient(circle at top left, rgba(34, 211, 238, 0.28), transparent 30%),
    radial-gradient(circle at top right, rgba(59, 130, 246, 0.22), transparent 28%),
    linear-gradient(160deg, #08111f 0%, #0f172a 48%, #111827 100%);
}

.hero {
  max-width: 1100px;
  margin: 0 auto 28px;
  padding: 28px;
  display: grid;
  grid-template-columns: minmax(0, 1.4fr) minmax(240px, 0.6fr);
  gap: 20px;
  border: 1px solid rgba(148, 163, 184, 0.18);
  border-radius: 28px;
  background: rgba(15, 23, 42, 0.72);
  box-shadow: 0 24px 80px rgba(2, 6, 23, 0.45);
  backdrop-filter: blur(18px);
}

.hero__content h1 {
  margin: 0;
  font-size: clamp(2rem, 4vw, 3.8rem);
  line-height: 0.98;
  letter-spacing: -0.04em;
}

.eyebrow {
  margin: 0 0 12px;
  text-transform: uppercase;
  letter-spacing: 0.28em;
  font-size: 0.75rem;
  color: #67e8f9;
}

.hero__text {
  max-width: 56ch;
  margin-top: 16px;
  color: #cbd5e1;
  font-size: 1.02rem;
}

.hero__stats {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 12px;
  margin-top: 24px;
}

.stat-card {
  padding: 16px;
  border-radius: 20px;
  background: linear-gradient(180deg, rgba(30, 41, 59, 0.96), rgba(15, 23, 42, 0.9));
  border: 1px solid rgba(148, 163, 184, 0.15);
}

.stat-card__label,
.product-id,
dt {
  display: block;
  color: #94a3b8;
  font-size: 0.82rem;
}

.stat-card strong {
  display: block;
  margin-top: 6px;
  font-size: 1.15rem;
  color: #f8fafc;
}

.hero__badge {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 10px;
  padding: 22px;
  border-radius: 24px;
  background:
    linear-gradient(160deg, rgba(34, 211, 238, 0.22), rgba(59, 130, 246, 0.08)),
    rgba(15, 23, 42, 0.82);
  border: 1px solid rgba(103, 232, 249, 0.16);
}

.hero__badge span {
  color: #67e8f9;
  text-transform: uppercase;
  letter-spacing: 0.24em;
  font-size: 0.72rem;
}

.hero__badge strong {
  font-size: 1.2rem;
  line-height: 1.3;
}

.inventory-grid {
  max-width: 1100px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 18px;
}

.product-card {
  padding: 22px;
  border-radius: 24px;
  background: rgba(15, 23, 42, 0.72);
  border: 1px solid rgba(148, 163, 184, 0.16);
  box-shadow: 0 18px 50px rgba(2, 6, 23, 0.28);
  backdrop-filter: blur(14px);
}

.product-card__top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
}

.product-id {
  letter-spacing: 0.14em;
}

.status-pill {
  padding: 8px 12px;
  border-radius: 999px;
  font-size: 0.78rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.status-pill--disponible {
  color: #22c55e;
  background: rgba(34, 197, 94, 0.12);
}

.status-pill--stock-medio {
  color: #fbbf24;
  background: rgba(251, 191, 36, 0.12);
}

.status-pill--bajo-stock {
  color: #fb7185;
  background: rgba(251, 113, 133, 0.12);
}

.product-card h2 {
  margin: 16px 0 18px;
  font-size: 1.35rem;
  line-height: 1.2;
}

.product-meta {
  margin: 0;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 14px;
}

.product-meta div {
  padding: 14px;
  border-radius: 18px;
  background: rgba(30, 41, 59, 0.72);
}

dd {
  margin: 6px 0 0;
  color: #f8fafc;
  font-size: 1.08rem;
  font-weight: 700;
}

.product-progress {
  height: 10px;
  margin-top: 18px;
  overflow: hidden;
  border-radius: 999px;
  background: rgba(148, 163, 184, 0.18);
}

.product-progress__bar {
  height: 100%;
  border-radius: inherit;
  background: linear-gradient(90deg, #22d3ee, #3b82f6, #8b5cf6);
}

@media (max-width: 860px) {
  .hero,
  .inventory-grid {
    grid-template-columns: 1fr;
  }

  .hero__stats {
    grid-template-columns: 1fr;
  }
}
</style>