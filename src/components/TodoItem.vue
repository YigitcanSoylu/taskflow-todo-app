```vue
<template>
  <article
    class="task-card group rounded-xl border p-4 transition-all duration-200"
    :class="[
      isLight
        ? 'border-gray-200 bg-white hover:border-violet-200 hover:shadow-md hover:shadow-violet-500/5'
        : 'border-white/[0.06] bg-white/[0.025] hover:border-violet-400/20 hover:bg-white/[0.04]',
      todo.completed ? 'task-completed' : '',
      isOverdue ? 'task-overdue' : '',
    ]"
  >
    <div class="flex items-start gap-3">
      <!-- Tamamlandı işareti -->
      <button
        type="button"
        @click="$emit('toggle', todo.id)"
        class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-md border transition-all duration-200"
        :class="
          todo.completed
            ? 'border-emerald-500 bg-emerald-500 text-white'
            : isLight
              ? 'border-gray-300 bg-white hover:border-violet-500'
              : 'border-white/20 bg-transparent hover:border-violet-400'
        "
        :aria-label="todo.completed ? 'Görevi tamamlanmadı olarak işaretle' : 'Görevi tamamla'"
        :aria-pressed="todo.completed"
      >
        <svg
          v-if="todo.completed"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="none"
          class="h-3.5 w-3.5"
        >
          <path
            d="M4 10.5L8 14L16 6"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
      </button>

      <!-- Görev bilgileri -->
      <div class="min-w-0 flex-1">
        <button
          type="button"
          class="block w-full break-words text-left text-sm font-semibold leading-relaxed transition-colors"
          :class="
            todo.completed
              ? isLight
                ? 'text-gray-400 line-through'
                : 'text-gray-500 line-through'
              : isLight
                ? 'text-gray-800 hover:text-violet-700'
                : 'text-gray-200 hover:text-violet-300'
          "
          @click="$emit('details', todo)"
          :title="todo.text"
        >
          {{ todo.text }}
        </button>

        <!-- Etiketler -->
        <div class="mt-3 flex flex-wrap items-center gap-2">
          <span
            class="inline-flex items-center gap-1.5 rounded-md border px-2 py-1 text-[10px] font-semibold"
            :class="priorityClass"
          >
            <span class="h-1.5 w-1.5 rounded-full" :class="priorityDot"></span>
            {{ priorityLabel }}
          </span>

          <span
            v-if="todo.category"
            class="inline-flex items-center rounded-md border px-2 py-1 text-[10px] font-medium"
            :class="
              isLight
                ? 'border-blue-200 bg-blue-50 text-blue-700'
                : 'border-blue-400/15 bg-blue-500/10 text-blue-300'
            "
          >
            {{ todo.category }}
          </span>
        </div>

        <!-- Son tarih -->
        <div
          v-if="todo.deadline"
          class="mt-3 flex items-center gap-1.5 text-xs"
          :class="
            isOverdue
              ? 'text-red-400'
              : isLight
                ? 'text-gray-500'
                : 'text-gray-500'
          "
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.7"
            class="h-3.5 w-3.5 shrink-0"
          >
            <rect x="3" y="5" width="18" height="16" rx="2" />
            <path d="M16 3V7M8 3V7M3 11H21" />
          </svg>

          <span>{{ formattedDeadline }}</span>

          <span
            v-if="isOverdue"
            class="rounded px-1.5 py-0.5 text-[10px] font-semibold"
            :class="
              isLight
                ? 'bg-red-50 text-red-600'
                : 'bg-red-500/10 text-red-400'
            "
          >
            Gecikti
          </span>
        </div>
      </div>

      <!-- İşlem butonları -->
      <div class="flex shrink-0 items-center gap-1">
        <button
          type="button"
          @click="$emit('edit', todo)"
          class="flex h-8 w-8 items-center justify-center rounded-lg transition-all"
          :class="
            isLight
              ? 'text-gray-400 hover:bg-violet-50 hover:text-violet-700'
              : 'text-gray-500 hover:bg-violet-500/10 hover:text-violet-300'
          "
          title="Görevi düzenle"
          aria-label="Görevi düzenle"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.7"
            class="h-4 w-4"
          >
            <path
              d="M12 20H21"
              stroke-linecap="round"
            />
            <path
              d="M16.5 3.5A2.12 2.12 0 0 1 19.5 6.5L8 18L4 19L5 15L16.5 3.5Z"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </button>

        <button
          type="button"
          @click="$emit('delete', todo.id)"
          class="flex h-8 w-8 items-center justify-center rounded-lg transition-all"
          :class="
            isLight
              ? 'text-gray-400 hover:bg-red-50 hover:text-red-600'
              : 'text-gray-500 hover:bg-red-500/10 hover:text-red-400'
          "
          title="Görevi sil"
          aria-label="Görevi sil"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="1.7"
            class="h-4 w-4"
          >
            <path
              d="M3 6H21"
              stroke-linecap="round"
            />
            <path
              d="M8 6V4H16V6M19 6L18 20H6L5 6"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
            <path
              d="M10 10V16M14 10V16"
              stroke-linecap="round"
            />
          </svg>
        </button>
      </div>
    </div>
  </article>
</template>

<script setup>
import { computed } from 'vue'
import { useTodoStore } from '../stores/todoStore.js'

const props = defineProps({
  todo: {
    type: Object,
    required: true,
  },
})

defineEmits(['toggle', 'edit', 'delete', 'details'])

const store = useTodoStore()

const isLight = computed(() => store.theme === 'light')

const priorityLabel = computed(() => {
  const labels = {
    low: 'Düşük öncelik',
    medium: 'Orta öncelik',
    high: 'Yüksek öncelik',
  }

  return labels[props.todo.priority] || 'Normal'
})

const priorityClass = computed(() => {
  const classes = {
    low: isLight.value
      ? 'border-emerald-200 bg-emerald-50 text-emerald-700'
      : 'border-emerald-400/15 bg-emerald-500/10 text-emerald-300',

    medium: isLight.value
      ? 'border-amber-200 bg-amber-50 text-amber-700'
      : 'border-amber-400/15 bg-amber-500/10 text-amber-300',

    high: isLight.value
      ? 'border-red-200 bg-red-50 text-red-700'
      : 'border-red-400/15 bg-red-500/10 text-red-300',
  }

  return classes[props.todo.priority] || classes.medium
})

const priorityDot = computed(() => {
  const dots = {
    low: 'bg-emerald-400',
    medium: 'bg-amber-400',
    high: 'bg-red-400',
  }

  return dots[props.todo.priority] || dots.medium
})

const isOverdue = computed(() => {
  if (!props.todo.deadline || props.todo.completed) return false

  const deadline = new Date(props.todo.deadline)
  const today = new Date()

  today.setHours(0, 0, 0, 0)
  deadline.setHours(0, 0, 0, 0)

  return deadline < today
})

const formattedDeadline = computed(() => {
  if (!props.todo.deadline) return ''

  const date = new Date(props.todo.deadline)

  if (Number.isNaN(date.getTime())) return props.todo.deadline

  return new Intl.DateTimeFormat('tr-TR', {
    day: 'numeric',
    month: 'short',
    year: 'numeric',
  }).format(date)
})
</script>

<style scoped>
.task-card {
  min-width: 0;
}

.task-card:hover {
  transform: translateY(-1px);
}

.task-completed {
  opacity: 0.8;
}

.task-overdue {
  border-left: 2px solid rgba(248, 113, 113, 0.7);
}

@media (prefers-reduced-motion: reduce) {
  .task-card {
    transition: none;
  }

  .task-card:hover {
    transform: none;
  }
}
</style>
```
