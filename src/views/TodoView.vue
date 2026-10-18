<template>
  <div
    class="todo-page min-h-screen px-4 py-10 sm:px-6 lg:px-8"
    :class="store.theme === 'light' ? 'todo-light' : 'todo-dark'"
  >
    <div class="mx-auto max-w-7xl">
      <!-- Sayfa başlığı -->
      <header
        class="mb-10 flex flex-col justify-between gap-5 sm:flex-row sm:items-end"
      >
        <div>
          <div
            class="mb-3 inline-flex items-center gap-2 rounded-full border px-3 py-1.5 text-xs font-medium"
            :class="
              isLight
                ? 'border-violet-200 bg-violet-50 text-violet-700'
                : 'border-violet-400/20 bg-violet-500/10 text-violet-300'
            "
          >
            <span class="h-2 w-2 rounded-full bg-emerald-400"></span>
            Kişisel çalışma alanın
          </div>

          <h1
            class="text-3xl font-bold tracking-tight sm:text-4xl"
            :class="isLight ? 'text-gray-900' : 'text-white'"
          >
            Görevlerini yönet<span class="text-violet-400">.</span>
          </h1>

          <p
            class="mt-2 text-sm sm:text-base"
            :class="isLight ? 'text-gray-500' : 'text-gray-400'"
          >
            Planını oluştur, önceliklerini belirle ve hedeflerine ulaş.
          </p>
        </div>

        <div
          class="flex items-center gap-3 rounded-2xl border px-4 py-3"
          :class="surfaceClass"
        >
          <div
            class="flex h-11 w-11 items-center justify-center rounded-xl bg-violet-500/15 text-xl"
          >
            ⚡
          </div>

          <div>
            <p
              class="text-xs"
              :class="mutedTextClass"
            >
              Tamamlanma oranı
            </p>

            <p
              class="text-xl font-bold"
              :class="textClass"
            >
              {{ completionRate }}%
            </p>
          </div>

          <div
            class="ml-2 h-10 w-10 rounded-full p-1"
            :style="{
              background: `conic-gradient(#8b5cf6 ${completionRate * 3.6}deg, ${
                isLight ? '#e5e7eb' : '#292633'
              } 0deg)`,
            }"
          >
            <div
              class="flex h-full w-full items-center justify-center rounded-full text-[10px] font-bold"
              :class="isLight ? 'bg-white text-violet-700' : 'bg-[#151821] text-violet-300'"
            >
              {{ completionRate }}%
            </div>
          </div>
        </div>
      </header>

      <!-- İstatistik kartları -->
      <section class="mb-8 grid grid-cols-2 gap-4 xl:grid-cols-4">
        <article
          v-for="stat in stats"
          :key="stat.label"
          class="stat-card rounded-2xl border p-5 sm:p-6"
          :class="surfaceClass"
        >
          <div class="flex items-start justify-between gap-3">
            <div>
              <p
                class="text-sm"
                :class="mutedTextClass"
              >
                {{ stat.label }}
              </p>

              <p
                class="mt-3 text-3xl font-bold tracking-tight sm:text-4xl"
                :class="stat.valueClass"
              >
                {{ stat.value }}
              </p>
            </div>

            <div
              class="flex h-11 w-11 shrink-0 items-center justify-center rounded-xl text-xl"
              :class="stat.iconBg"
            >
              {{ stat.icon }}
            </div>
          </div>

          <div class="mt-5 flex items-center gap-2">
            <div
              class="h-1.5 flex-1 overflow-hidden rounded-full"
              :class="isLight ? 'bg-gray-100' : 'bg-white/5'"
            >
              <div
                class="h-full rounded-full transition-all duration-500"
                :class="stat.barClass"
                :style="{ width: stat.percentage + '%' }"
              ></div>
            </div>

            <span
              class="text-xs"
              :class="mutedTextClass"
            >
              {{ stat.percentage }}%
            </span>
          </div>
        </article>
      </section>

      <!-- Ana içerik -->
      <div class="grid grid-cols-1 items-start gap-6 lg:grid-cols-12">
        <!-- Görev listesi -->
        <section class="min-w-0 lg:col-span-8">
          <div
            class="rounded-2xl border p-4 sm:p-6"
            :class="surfaceClass"
          >
            <!-- Liste başlığı -->
            <div
              class="mb-6 flex flex-wrap items-center justify-between gap-3"
            >
              <div>
                <h2
                  class="text-lg font-bold"
                  :class="textClass"
                >
                  Görev listem
                </h2>

                <p
                  class="mt-1 text-xs"
                  :class="mutedTextClass"
                >
                  Görevlerini görüntüle ve durumlarını takip et.
                </p>
              </div>

              <span
                class="rounded-lg border px-3 py-1.5 text-xs font-medium"
                :class="
                  isLight
                    ? 'border-violet-200 bg-violet-50 text-violet-700'
                    : 'border-violet-400/20 bg-violet-500/10 text-violet-300'
                "
              >
                {{ filteredTodos.length }} görev
              </span>
            </div>

            <!-- Arama -->
            <div class="relative mb-4">
              <span
                class="pointer-events-none absolute left-4 top-1/2 -translate-y-1/2 text-base"
                :class="mutedTextClass"
              >
                ⌕
              </span>

              <input
                v-model="searchQuery"
                type="search"
                placeholder="Görevlerinde ara..."
                class="w-full rounded-xl border py-3 pl-11 pr-4 text-sm outline-none transition-all focus:border-violet-500 focus:ring-2 focus:ring-violet-500/10"
                :class="inputClass"
                aria-label="Görevlerde ara"
              />
            </div>

            <!-- Durum filtreleri -->
            <div class="mb-5 flex flex-wrap gap-2">
              <button
                v-for="filter in filters"
                :key="filter.value"
                @click="activeFilter = filter.value"
                class="rounded-lg border px-3 py-2 text-xs font-medium transition-all"
                :class="
                  activeFilter === filter.value
                    ? 'border-violet-500 bg-violet-500 text-white shadow-lg shadow-violet-500/10'
                    : filterClass
                "
              >
                {{ filter.label }}
              </button>
            </div>

            <!-- Kategori filtreleri -->
            <div
              v-if="store.categories.length > 0"
              class="mb-5 border-b pb-5"
              :class="isLight ? 'border-gray-100' : 'border-white/5'"
            >
              <p
                class="mb-3 text-xs font-medium"
                :class="mutedTextClass"
              >
                Kategoriler
              </p>

              <div class="flex flex-wrap gap-2">
                <button
                  @click="activeCategory = 'all'"
                  class="rounded-full border px-3 py-1.5 text-xs transition-all"
                  :class="
                    activeCategory === 'all'
                      ? 'border-blue-400/40 bg-blue-500/15 text-blue-400'
                      : filterClass
                  "
                >
                  Tümü
                </button>

                <button
                  v-for="category in store.categories"
                  :key="category"
                  @click="activeCategory = category"
                  class="rounded-full border px-3 py-1.5 text-xs transition-all"
                  :class="
                    activeCategory === category
                      ? 'border-blue-400/40 bg-blue-500/15 text-blue-400'
                      : filterClass
                  "
                >
                  {{ category }}
                </button>
              </div>
            </div>

            <!-- Görev listesi -->
            <div
              v-if="filteredTodos.length > 0"
              class="space-y-3"
            >
              <draggable
                v-model="draggableTodos"
                item-key="id"
                handle=".drag-handle"
                animation="200"
                ghost-class="task-ghost"
                class="space-y-3"
              >
                <template #item="{ element }">
                  <div class="task-row flex min-w-0 items-start gap-2">
                    <button
                      type="button"
                      class="drag-handle mt-4 shrink-0 cursor-grab select-none px-1 text-lg transition-colors active:cursor-grabbing"
                      :class="
                        isLight
                          ? 'text-gray-300 hover:text-violet-500'
                          : 'text-gray-600 hover:text-violet-300'
                      "
                      title="Görevi sürükleyerek sırala"
                      aria-label="Görevi sürükleyerek sırala"
                    >
                      ⠿
                    </button>

                    <div class="min-w-0 flex-1">
                      <TodoItem
                        :todo="element"
                        @toggle="store.toggleTodo"
                        @edit="startEdit"
                        @delete="store.deleteTodo"
                      />
                    </div>
                  </div>
                </template>
              </draggable>

              <!-- Tamamlanan görevleri temizle -->
              <button
                v-if="store.completedCount > 0"
                @click="clearCompleted"
                class="mt-3 w-full rounded-xl border border-transparent py-3 text-xs font-medium transition-all"
                :class="
                  isLight
                    ? 'text-gray-500 hover:border-red-100 hover:bg-red-50 hover:text-red-600'
                    : 'text-gray-500 hover:border-red-400/10 hover:bg-red-500/5 hover:text-red-400'
                "
              >
                ✓ Tamamlanan görevleri temizle
              </button>
            </div>

            <!-- Boş durum -->
            <div
              v-else
              class="flex flex-col items-center justify-center rounded-2xl border border-dashed px-5 py-14 text-center"
              :class="isLight ? 'border-gray-200' : 'border-white/10'"
            >
              <div
                class="mb-4 flex h-16 w-16 items-center justify-center rounded-2xl text-3xl"
                :class="isLight ? 'bg-violet-50' : 'bg-violet-500/10'"
              >
                {{ searchQuery ? '🔎' : '📝' }}
              </div>

              <h3
                class="font-semibold"
                :class="textClass"
              >
                {{
                  searchQuery || activeFilter !== 'all' || activeCategory !== 'all'
                    ? 'Görev bulunamadı'
                    : 'Henüz görevin yok'
                }}
              </h3>

              <p
                class="mt-2 max-w-xs text-sm"
                :class="mutedTextClass"
              >
                {{
                  searchQuery || activeFilter !== 'all' || activeCategory !== 'all'
                    ? 'Arama kelimeni veya filtrelerini değiştirmeyi deneyebilirsin.'
                    : 'Yeni bir görev oluşturarak verimli gününe başlayabilirsin.'
                }}
              </p>

              <button
                v-if="searchQuery || activeFilter !== 'all' || activeCategory !== 'all'"
                @click="resetFilters"
                class="mt-5 rounded-xl bg-violet-500 px-4 py-2.5 text-xs font-semibold text-white transition-colors hover:bg-violet-600"
              >
                Filtreleri temizle
              </button>
            </div>
          </div>
        </section>

        <!-- Sağ panel -->
        <aside class="space-y-6 lg:col-span-4">
          <!-- Görev formu -->
          <section
            class="rounded-2xl border p-5 sm:p-6"
            :class="surfaceClass"
          >
            <div class="mb-5 flex items-center gap-3">
              <div
                class="flex h-10 w-10 items-center justify-center rounded-xl bg-violet-500/15 text-lg"
              >
                {{ editingTodo ? '✏️' : '✚' }}
              </div>

              <div>
                <h2
                  class="font-bold"
                  :class="textClass"
                >
                  {{ editingTodo ? 'Görevi düzenle' : 'Yeni görev oluştur' }}
                </h2>

                <p
                  class="mt-1 text-xs"
                  :class="mutedTextClass"
                >
                  {{ editingTodo ? 'Bilgileri güncelle.' : 'Planına yeni bir hedef ekle.' }}
                </p>
              </div>
            </div>

            <TodoForm
              :editingTodo="editingTodo"
              @add="handleAdd"
              @update="handleUpdate"
              @cancel="cancelEdit"
            />
          </section>

          <!-- Öncelik özeti -->
          <section
            class="rounded-2xl border p-5 sm:p-6"
            :class="surfaceClass"
          >
            <div class="mb-5">
              <h2
                class="font-bold"
                :class="textClass"
              >
                Öncelik dağılımı
              </h2>

              <p
                class="mt-1 text-xs"
                :class="mutedTextClass"
              >
                Görevlerinin öncelik seviyeleri.
              </p>
            </div>

            <div class="space-y-5">
              <div
                v-for="priority in priorities"
                :key="priority.value"
              >
                <div class="mb-2 flex items-center justify-between">
                  <div class="flex items-center gap-2">
                    <span
                      class="h-2 w-2 rounded-full"
                      :class="priority.dot"
                    ></span>

                    <span
                      class="text-sm"
                      :class="isLight ? 'text-gray-600' : 'text-gray-300'"
                    >
                      {{ priority.label }}
                    </span>
                  </div>

                  <span
                    class="text-sm font-semibold"
                    :class="textClass"
                  >
                    {{ priorityCount(priority.value) }}
                  </span>
                </div>

                <div
                  class="h-2 overflow-hidden rounded-full"
                  :class="isLight ? 'bg-gray-100' : 'bg-white/5'"
                >
                  <div
                    class="h-full rounded-full transition-all duration-500"
                    :class="priority.bar"
                    :style="{ width: priorityRate(priority.value) + '%' }"
                  ></div>
                </div>
              </div>
            </div>
          </section>

          <!-- Küçük motivasyon kartı -->
          <section
            class="relative overflow-hidden rounded-2xl border border-violet-400/20 bg-gradient-to-br from-violet-600/20 via-violet-500/10 to-transparent p-5 sm:p-6"
          >
            <div
              class="pointer-events-none absolute -right-8 -top-8 h-28 w-28 rounded-full bg-violet-500/10 blur-2xl"
            ></div>

            <div class="relative">
              <span class="text-2xl">🎯</span>

              <h3
                class="mt-3 font-bold"
                :class="textClass"
              >
                Küçük adımlar, büyük hedefler.
              </h3>

              <p
                class="mt-2 text-sm leading-relaxed"
                :class="mutedTextClass"
              >
                Her tamamladığın görev, hedeflerine bir adım daha yaklaşmanı sağlar.
              </p>

              <div
                class="mt-5 h-1.5 overflow-hidden rounded-full"
                :class="isLight ? 'bg-violet-100' : 'bg-white/10'"
              >
                <div
                  class="h-full rounded-full bg-gradient-to-r from-violet-500 to-fuchsia-400 transition-all duration-500"
                  :style="{ width: completionRate + '%' }"
                ></div>
              </div>

              <p
                class="mt-2 text-right text-xs"
                :class="mutedTextClass"
              >
                {{ store.completedCount }} / {{ store.todos.length }} görev tamamlandı
              </p>
            </div>
          </section>
        </aside>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
import draggable from 'vuedraggable'
import TodoForm from '../components/TodoForm.vue'
import TodoItem from '../components/TodoItem.vue'
import { useTodoStore } from '../stores/todoStore.js'

const store = useTodoStore()

const editingTodo = ref(null)
const searchQuery = ref('')
const activeFilter = ref('all')
const activeCategory = ref('all')

const isLight = computed(() => store.theme === 'light')

const textClass = computed(() =>
  isLight.value ? 'text-gray-900' : 'text-white',
)

const mutedTextClass = computed(() =>
  isLight.value ? 'text-gray-500' : 'text-gray-400',
)

const surfaceClass = computed(() =>
  isLight.value
    ? 'border-gray-200 bg-white shadow-sm'
    : 'border-white/[0.07] bg-[#151821]/90 shadow-xl shadow-black/5',
)

const inputClass = computed(() =>
  isLight.value
    ? 'border-gray-200 bg-gray-50 text-gray-900 placeholder:text-gray-400'
    : 'border-white/10 bg-white/[0.035] text-white placeholder:text-gray-500',
)

const filterClass = computed(() =>
  isLight.value
    ? 'border-gray-200 bg-white text-gray-500 hover:border-violet-200 hover:text-violet-700'
    : 'border-white/[0.07] bg-white/[0.025] text-gray-400 hover:bg-white/[0.06] hover:text-gray-200',
)

const filters = [
  { label: 'Tümü', value: 'all' },
  { label: 'Devam eden', value: 'active' },
  { label: 'Tamamlanan', value: 'completed' },
  { label: 'Geciken', value: 'overdue' },
]

const priorities = [
  {
    value: 'high',
    label: 'Yüksek öncelik',
    dot: 'bg-red-400',
    bar: 'bg-red-400',
  },
  {
    value: 'medium',
    label: 'Orta öncelik',
    dot: 'bg-amber-400',
    bar: 'bg-amber-400',
  },
  {
    value: 'low',
    label: 'Düşük öncelik',
    dot: 'bg-emerald-400',
    bar: 'bg-emerald-400',
  },
]

const completionRate = computed(() => {
  if (store.todos.length === 0) return 0

  return Math.round(
    (store.completedCount / store.todos.length) * 100,
  )
})

const stats = computed(() => {
  const total = store.todos.length

  return [
    {
      label: 'Toplam görev',
      value: total,
      icon: '▤',
      iconBg: 'bg-violet-500/15 text-violet-400',
      valueClass: isLight.value ? 'text-gray-900' : 'text-white',
      barClass: 'bg-violet-500',
      percentage: total > 0 ? 100 : 0,
    },
    {
      label: 'Tamamlanan',
      value: store.completedCount,
      icon: '✓',
      iconBg: 'bg-emerald-500/15 text-emerald-400',
      valueClass: 'text-emerald-400',
      barClass: 'bg-emerald-400',
      percentage: total > 0
        ? Math.round((store.completedCount / total) * 100)
        : 0,
    },
    {
      label: 'Devam eden',
      value: store.remainingCount,
      icon: '◷',
      iconBg: 'bg-blue-500/15 text-blue-400',
      valueClass: 'text-blue-400',
      barClass: 'bg-blue-400',
      percentage: total > 0
        ? Math.round((store.remainingCount / total) * 100)
        : 0,
    },
    {
      label: 'Geciken',
      value: store.overdueCount,
      icon: '!',
      iconBg: 'bg-red-500/15 text-red-400',
      valueClass: 'text-red-400',
      barClass: 'bg-red-400',
      percentage: total > 0
        ? Math.round((store.overdueCount / total) * 100)
        : 0,
    },
  ]
})

const filteredTodos = computed(() => {
  let list = store.todos

  const query = searchQuery.value.trim().toLocaleLowerCase('tr-TR')

  if (query) {
    list = list.filter((todo) =>
      todo.text.toLocaleLowerCase('tr-TR').includes(query),
    )
  }

  if (activeFilter.value === 'active') {
    list = list.filter((todo) => !todo.completed)
  } else if (activeFilter.value === 'completed') {
    list = list.filter((todo) => todo.completed)
  } else if (activeFilter.value === 'overdue') {
    list = list.filter(
      (todo) =>
        !todo.completed &&
        todo.deadline &&
        new Date(todo.deadline) < new Date(),
    )
  }

  if (activeCategory.value !== 'all') {
    list = list.filter(
      (todo) => todo.category === activeCategory.value,
    )
  }

  return list
})

/*
 * Filtrelenmiş bir listeyi sürükleyerek sıraladığımızda,
 * görünmeyen görevlerin listedeki yerlerini koruruz.
 */
const draggableTodos = computed({
  get: () => filteredTodos.value,

  set: (newList) => {
    const visibleIds = new Set(
      filteredTodos.value.map((todo) => todo.id),
    )

    let index = 0

    const reordered = store.todos.map((todo) => {
      if (visibleIds.has(todo.id)) {
        return newList[index++] ?? todo
      }

      return todo
    })

    store.reorderTodos(reordered)
  },
})

const priorityCount = (priority) =>
  store.todos.filter((todo) => todo.priority === priority).length

const priorityRate = (priority) => {
  if (store.todos.length === 0) return 0

  return Math.round(
    (priorityCount(priority) / store.todos.length) * 100,
  )
}

const startEdit = (todo) => {
  editingTodo.value = { ...todo }

  window.scrollTo({
    top: 0,
    behavior: 'smooth',
  })
}

const cancelEdit = () => {
  editingTodo.value = null
}

const handleAdd = (data) => {
  store.addTodo(data)
}

const handleUpdate = (data) => {
  if (!editingTodo.value) return

  store.updateTodo(editingTodo.value.id, data)
  editingTodo.value = null
}

const clearCompleted = () => {
  const confirmed = window.confirm(
    'Tamamlanan bütün görevleri silmek istediğine emin misin?',
  )

  if (confirmed) {
    store.clearCompleted()
  }
}

const resetFilters = () => {
  searchQuery.value = ''
  activeFilter.value = 'all'
  activeCategory.value = 'all'
}
</script>

<style scoped>
.todo-page {
  transition:
    background-color 0.25s ease,
    color 0.25s ease;
}

.stat-card {
  transition:
    transform 0.2s ease,
    border-color 0.2s ease,
    box-shadow 0.2s ease;
}

.stat-card:hover {
  transform: translateY(-2px);
}

.todo-dark .stat-card:hover {
  border-color: rgba(167, 139, 250, 0.2);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12);
}

.todo-light .stat-card:hover {
  border-color: rgba(139, 92, 246, 0.25);
  box-shadow: 0 12px 30px rgba(31, 25, 70, 0.05);
}

.task-ghost {
  opacity: 0.35;
  border-radius: 12px;
}

.task-row {
  animation: task-appear 0.25s ease both;
}

@keyframes task-appear {
  from {
    opacity: 0;
    transform: translateY(5px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (prefers-reduced-motion: reduce) {
  .stat-card,
  .task-row {
    animation: none;
    transition: none;
  }
}
</style>