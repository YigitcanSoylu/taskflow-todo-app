<script setup>
import { ref, watch, nextTick } from 'vue'
import { useTodoStore } from '../stores/todoStore.js'

const store = useTodoStore()

const props = defineProps({
  editingTodo: {
    type: Object,
    default: null,
  },
})

const emit = defineEmits(['add', 'update', 'cancel'])

const titleInput = ref(null)
const text = ref('')
const priority = ref('medium')
const category = ref('')
const deadline = ref('')
const errorMessage = ref('')

const isEditing = ref(false)

watch(
  () => props.editingTodo,
  async (todo) => {
    errorMessage.value = ''

    if (todo) {
      isEditing.value = true
      text.value = todo.text || ''
      priority.value = todo.priority || 'medium'
      category.value = todo.category || ''
      deadline.value = todo.deadline || ''

      await nextTick()
      titleInput.value?.focus()
    } else {
      resetForm()
    }
  },
  { immediate: true },
)

function resetForm() {
  text.value = ''
  priority.value = 'medium'
  category.value = ''
  deadline.value = ''
  errorMessage.value = ''
  isEditing.value = false
}

function submitForm() {
  errorMessage.value = ''

  const trimmedText = text.value.trim()
  const trimmedCategory = category.value.trim()

  if (!trimmedText) {
    errorMessage.value = 'Lütfen görev başlığını gir.'
    return
  }

  if (trimmedText.length > 150) {
    errorMessage.value = 'Görev başlığı en fazla 150 karakter olabilir.'
    return
  }

  if (!trimmedCategory) {
    errorMessage.value = 'Lütfen bir kategori gir.'
    return
  }

  if (deadline.value) {
    const today = new Date()
    today.setHours(0, 0, 0, 0)

    const selectedDate = new Date(`${deadline.value}T00:00:00`)

    if (selectedDate < today) {
      errorMessage.value = 'Son tarih geçmiş bir tarih olamaz.'
      return
    }
  }

  const todoData = {
    text: trimmedText,
    priority: priority.value,
    category: trimmedCategory,
    deadline: deadline.value,
  }

  if (isEditing.value && props.editingTodo) {
    emit('update', {
      id: props.editingTodo.id,
      ...todoData,
    })
  } else {
    emit('add', todoData)
    resetForm()
  }
}

function cancelForm() {
  resetForm()
  emit('cancel')
}
</script>

<template>
  <form class="todo-form" @submit.prevent="submitForm">
    <div class="form-heading">
      <div class="form-icon">
        <svg
          v-if="!isEditing"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1.8"
          stroke-linecap="round"
          stroke-linejoin="round"
          aria-hidden="true"
        >
          <path d="M12 5v14M5 12h14" />
        </svg>

        <svg
          v-else
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="1.8"
          stroke-linecap="round"
          stroke-linejoin="round"
          aria-hidden="true"
        >
          <path d="M12 20h9" />
          <path
            d="M16.5 3.5a2.12 2.12 0 0 1 3 3L9 17l-4 1 1-4Z"
          />
        </svg>
      </div>

      <div>
        <h2 class="form-title">
          {{ isEditing ? 'Görevi düzenle' : 'Yeni görev oluştur' }}
        </h2>

        <p class="form-description">
          {{
            isEditing
              ? 'Görev bilgilerini güncelle.'
              : 'Yeni bir görev ekleyerek işe koyul.'
          }}
        </p>
      </div>
    </div>

    <div class="form-group">
      <label for="todo-title">Görev başlığı</label>

      <input
        id="todo-title"
        ref="titleInput"
        v-model="text"
        type="text"
        class="form-control"
        placeholder="Örn. Proje sunumunu hazırla"
        maxlength="150"
        autocomplete="off"
      />

      <div class="input-footer">
        <span class="input-hint">Kısa ve açıklayıcı bir başlık seç.</span>
        <span class="character-count">{{ text.length }}/150</span>
      </div>
    </div>

    <div class="form-group">
      <label for="todo-category">Kategori</label>

      <input
        id="todo-category"
        v-model="category"
        type="text"
        class="form-control"
        list="todo-categories"
        placeholder="Örn. İş, Eğitim, Kişisel"
        maxlength="40"
        autocomplete="off"
      />

      <datalist id="todo-categories">
        <option
          v-for="item in store.categories"
          :key="item"
          :value="item"
        />
        <option value="İş" />
        <option value="Eğitim" />
        <option value="Kişisel" />
        <option value="Sağlık" />
        <option value="Alışveriş" />
      </datalist>
    </div>

    <div class="form-row">
      <div class="form-group">
        <label for="todo-priority">Öncelik seviyesi</label>

        <select
          id="todo-priority"
          v-model="priority"
          class="form-control priority-select"
        >
          <option value="low">Düşük</option>
          <option value="medium">Orta</option>
          <option value="high">Yüksek</option>
        </select>
      </div>

      <div class="form-group">
        <label for="todo-deadline">Son tarih</label>

        <input
          id="todo-deadline"
          v-model="deadline"
          type="date"
          class="form-control date-input"
        />
      </div>
    </div>

    <div v-if="errorMessage" class="form-error" role="alert">
      <svg
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
        aria-hidden="true"
      >
        <circle cx="12" cy="12" r="10" />
        <path d="M12 8v4M12 16h.01" />
      </svg>

      <span>{{ errorMessage }}</span>
    </div>

    <div class="form-actions">
      <button
        v-if="isEditing"
        type="button"
        class="btn-secondary"
        @click="cancelForm"
      >
        İptal
      </button>

      <button type="submit" class="btn-primary">
        <svg
          v-if="!isEditing"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          aria-hidden="true"
        >
          <path d="M12 5v14M5 12h14" />
        </svg>

        <svg
          v-else
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          aria-hidden="true"
        >
          <path d="m5 12 4 4L19 6" />
        </svg>

        {{ isEditing ? 'Değişiklikleri kaydet' : 'Görev ekle' }}
      </button>
    </div>
  </form>
</template>

<style scoped>
.todo-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
  width: 100%;
}

.form-heading {
  display: flex;
  align-items: center;
  gap: 12px;
  padding-bottom: 17px;
  border-bottom: 1px solid var(--border);
}

.form-icon {
  display: grid;
  width: 42px;
  height: 42px;
  flex-shrink: 0;
  place-items: center;
  border: 1px solid rgba(139, 92, 246, 0.2);
  border-radius: 12px;
  background: rgba(139, 92, 246, 0.12);
  color: var(--accent);
}

.form-icon svg {
  width: 21px;
  height: 21px;
}

.form-title {
  margin: 0;
  color: var(--text-primary);
  font-size: 15px;
  font-weight: 700;
  letter-spacing: -0.3px;
}

.form-description {
  margin: 3px 0 0;
  color: var(--text-secondary);
  font-size: 12px;
}

.form-group {
  display: flex;
  min-width: 0;
  flex: 1;
  flex-direction: column;
  gap: 8px;
}

.form-group label {
  color: var(--text-primary);
  font-size: 12px;
  font-weight: 600;
}

.form-control {
  width: 100%;
  min-width: 0;
  min-height: 43px;
  padding: 10px 12px;
  border: 1px solid var(--border);
  border-radius: 10px;
  outline: none;
  background: var(--surface);
  color: var(--text-primary);
  font-size: 13px;
  transition:
    border-color 0.2s ease,
    box-shadow 0.2s ease,
    background-color 0.2s ease;
}

.form-control::placeholder {
  color: var(--text-secondary);
  opacity: 0.65;
}

.form-control:focus {
  border-color: var(--accent);
  box-shadow: 0 0 0 3px rgba(139, 92, 246, 0.12);
}

/* DARK MODE: Açılır menü seçeneklerinin yazı ve arka plan renkleri */
.priority-select {
  color-scheme: dark;
  cursor: pointer;
}

.priority-select option {
  background-color: #151821;
  color: #f4f4f5;
}

/* Tarih seçicinin koyu tema görünümü */
.date-input {
  color-scheme: dark;
}

.input-footer {
  display: flex;
  justify-content: space-between;
  gap: 8px;
}

.input-hint,
.character-count {
  color: var(--text-secondary);
  font-size: 10px;
}

.form-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
  gap: 12px;
}

.form-error {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  padding: 10px 12px;
  border: 1px solid rgba(248, 113, 113, 0.2);
  border-radius: 9px;
  background: rgba(248, 113, 113, 0.08);
  color: #f87171;
  font-size: 12px;
}

.form-error svg {
  width: 16px;
  height: 16px;
  flex-shrink: 0;
  margin-top: 1px;
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  padding-top: 2px;
}

.btn-primary,
.btn-secondary {
  display: inline-flex;
  min-height: 42px;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 15px;
  border: 1px solid transparent;
  border-radius: 10px;
  font-size: 12px;
  font-weight: 650;
  transition:
    background-color 0.2s ease,
    border-color 0.2s ease,
    transform 0.2s ease;
}

.btn-primary {
  background: var(--accent);
  color: #fff;
}

.btn-primary:hover {
  transform: translateY(-1px);
  background: var(--accent-hover);
}

.btn-secondary {
  border-color: var(--border);
  background: var(--surface);
  color: var(--text-secondary);
}

.btn-secondary:hover {
  background: var(--surface-hover);
  color: var(--text-primary);
}

.btn-primary svg,
.btn-secondary svg {
  width: 16px;
  height: 16px;
}

:global(.light-theme) .form-control {
  color-scheme: light;
  background: #fff;
  color: #18181b;
}

:global(.light-theme) .form-control option,
:global(.light-theme) .priority-select option {
  background-color: #fff;
  color: #18181b;
}

:global(.light-theme) .date-input {
  color-scheme: light;
}

:global(.light-theme) .btn-secondary {
  background: #fff;
}

@media (max-width: 480px) {
  .form-row {
    grid-template-columns: 1fr;
  }

  .todo-form {
    gap: 16px;
  }

  .form-actions {
    flex-wrap: wrap;
  }

  .btn-primary,
  .btn-secondary {
    flex: 1;
  }
}

@media (prefers-reduced-motion: reduce) {
  .form-control,
  .btn-primary,
  .btn-secondary {
    transition: none;
  }
}
</style>