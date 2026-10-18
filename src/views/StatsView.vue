<template>
  <div
    class="stats-page min-h-screen px-4 py-10 sm:px-6 lg:px-8"
    :class="isLight ? 'stats-light' : 'stats-dark'"
  >
    <div class="mx-auto max-w-7xl">
      <!-- Sayfa başlığı -->
      <header class="mb-10">
        <div
          class="mb-3 inline-flex items-center gap-2 rounded-full border px-3 py-1.5 text-xs font-medium"
          :class="
            isLight
              ? 'border-violet-200 bg-violet-50 text-violet-700'
              : 'border-violet-400/20 bg-violet-500/10 text-violet-300'
          "
        >
          <span>✦</span>
          Performans merkezi
        </div>

        <h1
          class="text-3xl font-bold tracking-tight sm:text-4xl"
          :class="textClass"
        >
          İstatistikler<span class="text-violet-400">.</span>
        </h1>

        <p class="mt-2 text-sm sm:text-base" :class="mutedClass">
          Verimliliğini takip et, ilerlemeni analiz et ve hedeflerine odaklan.
        </p>
      </header>

      <!-- Genel istatistikler -->
      <section class="mb-8 grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-4">
        <article
          v-for="stat in summaryStats"
          :key="stat.label"
          class="stat-card rounded-2xl border p-5 sm:p-6"
          :class="surfaceClass"
        >
          <div class="flex items-start justify-between gap-3">
            <div>
              <p class="text-sm" :class="mutedClass">
                {{ stat.label }}
              </p>

              <p
                class="mt-3 text-3xl font-bold tracking-tight"
                :class="stat.valueClass"
              >
                {{ stat.value }}
              </p>
            </div>

            <div
              class="flex h-11 w-11 items-center justify-center rounded-xl text-xl"
              :class="stat.iconClass"
            >
              {{ stat.icon }}
            </div>
          </div>

          <p class="mt-4 text-xs" :class="mutedClass">
            {{ stat.description }}
          </p>
        </article>
      </section>

      <!-- Grafikler -->
      <section class="mb-8 grid grid-cols-1 gap-6 lg:grid-cols-2">
        <!-- Tamamlanma oranı -->
        <article class="rounded-2xl border p-5 sm:p-7" :class="surfaceClass">
          <div class="mb-6 flex items-start justify-between gap-4">
            <div>
              <h2 class="font-bold" :class="textClass">
                Görev tamamlama oranı
              </h2>

              <p class="mt-1 text-xs" :class="mutedClass">
                Tamamlanan görevlerin genel görev sayısına oranı
              </p>
            </div>

            <span
              class="rounded-lg px-3 py-1.5 text-xs font-semibold"
              :class="
                isLight
                  ? 'bg-violet-50 text-violet-700'
                  : 'bg-violet-500/10 text-violet-300'
              "
            >
              {{ completionRate }}%
            </span>
          </div>

          <div class="flex flex-col items-center justify-center py-5">
            <div
              class="completion-ring flex h-48 w-48 items-center justify-center rounded-full p-4 sm:h-56 sm:w-56"
              :style="completionRingStyle"
            >
              <div
                class="flex h-full w-full flex-col items-center justify-center rounded-full"
                :class="isLight ? 'bg-white' : 'bg-[#151821]'"
              >
                <span class="text-4xl font-bold" :class="textClass">
                  {{ completionRate }}%
                </span>

                <span class="mt-2 text-xs" :class="mutedClass">
                  Tamamlandı
                </span>
              </div>
            </div>

            <div class="mt-7 flex flex-wrap justify-center gap-x-6 gap-y-3">
              <div class="flex items-center gap-2">
                <span class="h-2.5 w-2.5 rounded-full bg-violet-500"></span>
                <span class="text-xs" :class="mutedClass">
                  {{ store.completedCount }} tamamlanan
                </span>
              </div>

              <div class="flex items-center gap-2">
                <span
                  class="h-2.5 w-2.5 rounded-full"
                  :class="isLight ? 'bg-gray-200' : 'bg-gray-700'"
                ></span>
                <span class="text-xs" :class="mutedClass">
                  {{ store.remainingCount }} devam eden
                </span>
              </div>
            </div>
          </div>
        </article>

        <!-- Öncelik dağılımı -->
        <article class="rounded-2xl border p-5 sm:p-7" :class="surfaceClass">
          <div class="mb-7">
            <h2 class="font-bold" :class="textClass">
              Öncelik dağılımı
            </h2>

            <p class="mt-1 text-xs" :class="mutedClass">
              Görevlerinin öncelik seviyelerine göre dağılımı
            </p>
          </div>

          <div v-if="store.todos.length" class="space-y-7">
            <div
              v-for="priority in priorityStats"
              :key="priority.value"
            >
              <div class="mb-3 flex items-center justify-between gap-3">
                <div class="flex items-center gap-3">
                  <div
                    class="flex h-9 w-9 items-center justify-center rounded-xl text-sm"
                    :class="priority.iconClass"
                  >
                    {{ priority.icon }}
                  </div>

                  <div>
                    <p class="text-sm font-medium" :class="textClass">
                      {{ priority.label }}
                    </p>

                    <p class="mt-0.5 text-xs" :class="mutedClass">
                      {{ priority.count }} görev
                    </p>
                  </div>
                </div>

                <span class="text-sm font-semibold" :class="textClass">
                  {{ priority.percentage }}%
                </span>
              </div>

              <div
                class="h-2.5 overflow-hidden rounded-full"
                :class="isLight ? 'bg-gray-100' : 'bg-white/[0.05]'"
              >
                <div
                  class="h-full rounded-full transition-all duration-700"
                  :class="priority.barClass"
                  :style="{ width: `${priority.percentage}%` }"
                ></div>
              </div>
            </div>
          </div>

          <div v-else class="py-12 text-center">
            <div class="mb-3 text-3xl">📊</div>
            <p class="text-sm" :class="mutedClass">
              Öncelik dağılımını görmek için görev ekle.
            </p>
          </div>
        </article>
      </section>

      <!-- Kategori analizi -->
      <section class="mb-8">
        <article class="rounded-2xl border p-5 sm:p-7" :class="surfaceClass">
          <div class="mb-6 flex flex-col justify-between gap-3 sm:flex-row sm:items-center">
            <div>
              <h2 class="font-bold" :class="textClass">
                Kategori analizi
              </h2>

              <p class="mt-1 text-xs" :class="mutedClass">
                Görevlerinin kategorilere göre dağılımı ve tamamlanma durumu
              </p>
            </div>

            <span class="text-xs" :class="mutedClass">
              {{ categoryStats.length }} kategori
            </span>
          </div>

          <div
            v-if="categoryStats.length"
            class="grid grid-cols-1 gap-4 md:grid-cols-2 xl:grid-cols-3"
          >
            <div
              v-for="category in categoryStats"
              :key="category.name"
              class="rounded-xl border p-4 transition-colors"
              :class="
                isLight
                  ? 'border-gray-100 bg-gray-50/70 hover:border-violet-200'
                  : 'border-white/[0.06] bg-white/[0.02] hover:border-violet-400/20'
              "
            >
              <div class="mb-4 flex items-start justify-between gap-3">
                <div class="flex min-w-0 items-center gap-3">
                  <div
                    class="flex h-10 w-10 shrink-0 items-center justify-center rounded-xl"
                    :class="category.iconClass"
                  >
                    {{ category.icon }}
                  </div>

                  <div class="min-w-0">
                    <h3 class="truncate text-sm font-semibold" :class="textClass">
                      {{ category.name }}
                    </h3>

                    <p class="mt-1 text-xs" :class="mutedClass">
                      {{ category.total }} görev
                    </p>
                  </div>
                </div>

                <span
                  class="shrink-0 rounded-lg px-2 py-1 text-xs font-semibold"
                  :class="
                    isLight
                      ? 'bg-emerald-50 text-emerald-700'
                      : 'bg-emerald-500/10 text-emerald-300'
                  "
                >
                  {{ category.rate }}%
                </span>
              </div>

              <div
                class="h-2 overflow-hidden rounded-full"
                :class="isLight ? 'bg-gray-200' : 'bg-white/[0.06]'"
              >
                <div
                  class="h-full rounded-full bg-gradient-to-r from-violet-600 to-fuchsia-400 transition-all duration-500"
                  :style="{ width: `${category.rate}%` }"
                ></div>
              </div>

              <div class="mt-3 flex justify-between text-xs" :class="mutedClass">
                <span>{{ category.completed }} tamamlandı</span>
                <span>{{ category.remaining }} kaldı</span>
              </div>
            </div>
          </div>

          <div
            v-else
            class="rounded-xl border border-dashed px-5 py-12 text-center"
            :class="isLight ? 'border-gray-200' : 'border-white/10'"
          >
            <div class="mb-3 text-3xl">🏷️</div>

            <p class="font-medium" :class="textClass">
              Henüz kategori verisi yok
            </p>

            <p class="mt-2 text-sm" :class="mutedClass">
              Görevlerine kategori eklediğinde analizlerin burada görünecek.
            </p>
          </div>
        </article>
      </section>

      <!-- Son tarih analizi -->
      <section class="mb-8">
        <article class="rounded-2xl border p-5 sm:p-7" :class="surfaceClass">
          <div class="mb-6">
            <h2 class="font-bold" :class="textClass">
              Son tarih analizi
            </h2>

            <p class="mt-1 text-xs" :class="mutedClass">
              Zaman planlaman ve yaklaşan teslim tarihlerin
            </p>
          </div>

          <div class="grid grid-cols-1 gap-4 sm:grid-cols-3">
            <div
              v-for="item in deadlineStats"
              :key="item.label"
              class="rounded-xl border p-5"
              :class="
                isLight
                  ? 'border-gray-100 bg-gray-50/70'
                  : 'border-white/[0.05] bg-white/[0.02]'
              "
            >
              <div class="mb-4 flex items-center justify-between">
                <span class="text-sm" :class="mutedClass">
                  {{ item.label }}
                </span>

                <span class="text-xl">{{ item.icon }}</span>
              </div>

              <p class="text-3xl font-bold" :class="item.valueClass">
                {{ item.value }}
              </p>

              <p class="mt-2 text-xs" :class="mutedClass">
                {{ item.description }}
              </p>
            </div>
          </div>
        </article>
      </section>

      <!-- Alt bilgi -->
      <footer
        class="rounded-2xl border p-5 sm:p-6"
        :class="
          isLight
            ? 'border-violet-100 bg-violet-50/70'
            : 'border-violet-400/10 bg-violet-500/[0.06]'
        "
      >
        <div class="flex flex-col gap-4 sm:flex-row sm:items-center">
          <div
            class="flex h-12 w-12 shrink-0 items-center justify-center rounded-xl bg-violet-500/15 text-2xl"
          >
            💡
          </div>

          <div>
            <h3 class="font-semibold" :class="textClass">
              Küçük ilerlemeler de başarıdır.
            </h3>

            <p class="mt-1 text-sm leading-relaxed" :class="mutedClass">
              <template v-if="store.todos.length === 0">
                İlk görevini ekle ve kendi üretkenlik istatistiklerini oluşturmaya başla.
              </template>

              <template v-else-if="completionRate === 100">
                Harika iş! Bütün görevlerini tamamladın. Yeni hedefler belirleyebilirsin.
              </template>

              <template v-else>
                {{ store.completedCount }} görevi tamamladın. Bir sonraki görevine
                odaklanarak ilerlemeye devam edebilirsin.
              </template>
            </p>
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useTodoStore } from '../stores/todoStore.js'

const store = useTodoStore()

const isLight = computed(() => store.theme === 'light')

const textClass = computed(() =>
  isLight.value ? 'text-gray-900' : 'text-white',
)

const mutedClass = computed(() =>
  isLight.value ? 'text-gray-500' : 'text-gray-400',
)

const surfaceClass = computed(() =>
  isLight.value
    ? 'border-gray-200 bg-white shadow-sm'
    : 'border-white/[0.07] bg-[#151821]/90 shadow-xl shadow-black/5',
)

const completionRate = computed(() => {
  if (store.todos.length === 0) return 0

  return Math.round(
    (store.completedCount / store.todos.length) * 100,
  )
})

const completionRingStyle = computed(() => ({
  background: `conic-gradient(#8b5cf6 ${completionRate.value * 3.6}deg, ${
    isLight.value ? '#e5e7eb' : '#292633'
  } 0deg)`,
}))

const summaryStats = computed(() => [
  {
    label: 'Toplam görev',
    value: store.todos.length,
    icon: '▤',
    iconClass: 'bg-violet-500/15 text-violet-400',
    valueClass: textClass.value,
    description: 'Oluşturduğun tüm görevler',
  },
  {
    label: 'Tamamlanan',
    value: store.completedCount,
    icon: '✓',
    iconClass: 'bg-emerald-500/15 text-emerald-400',
    valueClass: 'text-emerald-400',
    description: 'Başarıyla bitirdiğin görevler',
  },
  {
    label: 'Devam eden',
    value: store.remainingCount,
    icon: '◷',
    iconClass: 'bg-blue-500/15 text-blue-400',
    valueClass: 'text-blue-400',
    description: 'Henüz tamamlanmamış görevler',
  },
  {
    label: 'Geciken',
    value: store.overdueCount,
    icon: '!',
    iconClass: 'bg-red-500/15 text-red-400',
    valueClass: 'text-red-400',
    description: 'Son tarihi geçmiş görevler',
  },
])

const priorityDefinitions = [
  {
    value: 'high',
    label: 'Yüksek öncelik',
    icon: '↑',
    iconClass: 'bg-red-500/10 text-red-400',
    barClass: 'bg-red-400',
  },
  {
    value: 'medium',
    label: 'Orta öncelik',
    icon: '↗',
    iconClass: 'bg-amber-500/10 text-amber-400',
    barClass: 'bg-amber-400',
  },
  {
    value: 'low',
    label: 'Düşük öncelik',
    icon: '↓',
    iconClass: 'bg-emerald-500/10 text-emerald-400',
    barClass: 'bg-emerald-400',
  },
]

const priorityStats = computed(() =>
  priorityDefinitions.map((priority) => {
    const count = store.todos.filter(
      (todo) => todo.priority === priority.value,
    ).length

    return {
      ...priority,
      count,
      percentage:
        store.todos.length > 0
          ? Math.round((count / store.todos.length) * 100)
          : 0,
    }
  }),
)

const categoryPalette = [
  { icon: '💼', iconClass: 'bg-blue-500/10 text-blue-400' },
  { icon: '📚', iconClass: 'bg-violet-500/10 text-violet-400' },
  { icon: '🎯', iconClass: 'bg-amber-500/10 text-amber-400' },
  { icon: '🌱', iconClass: 'bg-emerald-500/10 text-emerald-400' },
  { icon: '✨', iconClass: 'bg-pink-500/10 text-pink-400' },
  { icon: '📌', iconClass: 'bg-cyan-500/10 text-cyan-400' },
]

const categoryStats = computed(() => {
  const grouped = new Map()

  for (const todo of store.todos) {
    const name = todo.category?.trim() || 'Kategorisiz'

    if (!grouped.has(name)) {
      grouped.set(name, {
        name,
        total: 0,
        completed: 0,
        remaining: 0,
      })
    }

    const category = grouped.get(name)

    category.total += 1

    if (todo.completed) {
      category.completed += 1
    } else {
      category.remaining += 1
    }
  }

  return Array.from(grouped.values())
    .map((category, index) => ({
      ...category,
      rate:
        category.total > 0
          ? Math.round((category.completed / category.total) * 100)
          : 0,
      ...categoryPalette[index % categoryPalette.length],
    }))
    .sort((a, b) => b.total - a.total)
})

const deadlineStats = computed(() => {
  const today = new Date()
  today.setHours(0, 0, 0, 0)

  const tomorrow = new Date(today)
  tomorrow.setDate(tomorrow.getDate() + 1)

  const weekEnd = new Date(today)
  weekEnd.setDate(weekEnd.getDate() + 7)

  const pending = store.todos.filter((todo) => !todo.completed)

  const overdue = pending.filter((todo) => {
    if (!todo.deadline) return false

    const deadline = new Date(`${todo.deadline}T00:00:00`)

    return !Number.isNaN(deadline.getTime()) && deadline < today
  }).length

  const dueToday = pending.filter((todo) => {
    if (!todo.deadline) return false

    const deadline = new Date(`${todo.deadline}T00:00:00`)

    return (
      !Number.isNaN(deadline.getTime()) &&
      deadline >= today &&
      deadline < tomorrow
    )
  }).length

  const dueThisWeek = pending.filter((todo) => {
    if (!todo.deadline) return false

    const deadline = new Date(`${todo.deadline}T00:00:00`)

    return (
      !Number.isNaN(deadline.getTime()) &&
      deadline >= tomorrow &&
      deadline < weekEnd
    )
  }).length

  return [
    {
      label: 'Geciken görevler',
      value: overdue,
      icon: '⚠️',
      valueClass: 'text-red-400',
      description: 'Son tarihi geçmiş',
    },
    {
      label: 'Bugün bitirilecek',
      value: dueToday,
      icon: '📅',
      valueClass: 'text-amber-400',
      description: 'Son tarihi bugün olan',
    },
    {
      label: 'Bu hafta',
      value: dueThisWeek,
      icon: '🗓️',
      valueClass: 'text-blue-400',
      description: 'Önümüzdeki 7 gün içindeki',
    },
  ]
})
</script>

<style scoped>
.stats-page {
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

.stats-dark .stat-card:hover {
  border-color: rgba(167, 139, 250, 0.2);
}

.stats-light .stat-card:hover {
  border-color: rgba(139, 92, 246, 0.25);
  box-shadow: 0 12px 30px rgba(31, 25, 70, 0.05);
}

.completion-ring {
  transition: background 0.5s ease;
}

@media (prefers-reduced-motion: reduce) {
  .stat-card {
    transition: none;
  }

  .stat-card:hover {
    transform: none;
  }
}
</style>