```vue
<template>
  <div
    class="about-page min-h-screen px-4 py-10 sm:px-6 lg:px-8"
    :class="isLight ? 'about-light' : 'about-dark'"
  >
    <div class="mx-auto max-w-6xl">
      <!-- Hero -->
      <section
        class="hero-panel relative mb-8 overflow-hidden rounded-3xl border p-7 sm:p-12 lg:p-16"
        :class="
          isLight
            ? 'border-violet-100 bg-white'
            : 'border-white/[0.07] bg-[#151821]'
        "
      >
        <div
          class="pointer-events-none absolute -right-20 -top-20 h-80 w-80 rounded-full bg-violet-500/10 blur-3xl"
        ></div>

        <div
          class="pointer-events-none absolute -bottom-32 left-1/3 h-64 w-64 rounded-full bg-fuchsia-500/[0.07] blur-3xl"
        ></div>

        <div class="relative grid items-center gap-10 lg:grid-cols-[1.3fr_0.7fr]">
          <div>
            <div
              class="mb-5 inline-flex items-center gap-2 rounded-full border px-3 py-1.5 text-xs font-semibold"
              :class="
                isLight
                  ? 'border-violet-200 bg-violet-50 text-violet-700'
                  : 'border-violet-400/20 bg-violet-500/10 text-violet-300'
              "
            >
              <span class="h-2 w-2 rounded-full bg-emerald-400"></span>
              Proje hakkında
            </div>

            <h1
              class="text-4xl font-bold leading-tight tracking-tight sm:text-5xl lg:text-6xl"
              :class="textClass"
            >
              Planla.
              <span class="block text-violet-400">Odaklan.</span>
              Başar.
            </h1>

            <p
              class="mt-6 max-w-xl text-sm leading-7 sm:text-base"
              :class="mutedClass"
            >
              TaskFlow, günlük görevlerini düzenlemene, önceliklerini belirlemene
              ve ilerlemeni takip etmene yardımcı olmak için geliştirilmiş
              modern bir görev yönetim uygulamasıdır.
            </p>

            <div class="mt-8 flex flex-wrap gap-3">
              <RouterLink
                to="/"
                class="inline-flex items-center gap-2 rounded-xl bg-violet-600 px-5 py-3 text-sm font-semibold text-white transition-all hover:-translate-y-0.5 hover:bg-violet-500"
              >
                Görevlerime dön
                <span>→</span>
              </RouterLink>

              <RouterLink
                to="/stats"
                class="inline-flex items-center gap-2 rounded-xl border px-5 py-3 text-sm font-semibold transition-colors"
                :class="
                  isLight
                    ? 'border-gray-200 text-gray-700 hover:border-violet-200 hover:bg-violet-50'
                    : 'border-white/10 text-gray-300 hover:border-violet-400/30 hover:bg-white/5'
                "
              >
                İstatistikleri gör
              </RouterLink>
            </div>
          </div>

          <!-- Logo alanı -->
          <div class="flex items-center justify-center">
            <div class="logo-orbit relative flex h-56 w-56 items-center justify-center sm:h-64 sm:w-64">
              <div
                class="absolute inset-0 rounded-full border"
                :class="isLight ? 'border-violet-200' : 'border-violet-400/20'"
              ></div>

              <div
                class="absolute inset-5 rounded-full border"
                :class="isLight ? 'border-violet-200/70' : 'border-violet-400/15'"
              ></div>

              <div
                class="absolute inset-10 rounded-full border"
                :class="isLight ? 'border-violet-200/50' : 'border-violet-400/10'"
              ></div>

              <div
                class="relative flex h-28 w-28 items-center justify-center rounded-3xl bg-gradient-to-br from-violet-500 to-purple-700 text-6xl font-bold text-white shadow-2xl shadow-violet-600/30 sm:h-32 sm:w-32"
              >
                ✓
              </div>

              <div
                class="absolute right-7 top-7 flex h-10 w-10 items-center justify-center rounded-xl border text-lg shadow-lg"
                :class="
                  isLight
                    ? 'border-white bg-white text-emerald-500'
                    : 'border-white/10 bg-[#202330] text-emerald-400'
                "
              >
                ✓
              </div>

              <div
                class="absolute bottom-8 left-3 flex h-10 w-10 items-center justify-center rounded-xl border text-lg shadow-lg"
                :class="
                  isLight
                    ? 'border-white bg-white text-amber-500'
                    : 'border-white/10 bg-[#202330] text-amber-400'
                "
              >
                ✦
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Sayısal özet -->
      <section class="mb-8 grid grid-cols-2 gap-4 lg:grid-cols-4">
        <article
          v-for="metric in metrics"
          :key="metric.label"
          class="rounded-2xl border p-5"
          :class="surfaceClass"
        >
          <p class="text-xs" :class="mutedClass">
            {{ metric.label }}
          </p>

          <p class="mt-3 text-3xl font-bold" :class="metric.color">
            {{ metric.value }}
          </p>

          <p class="mt-2 text-xs" :class="mutedClass">
            {{ metric.description }}
          </p>
        </article>
      </section>

      <!-- Projenin amacı -->
      <section class="mb-8 grid grid-cols-1 gap-6 lg:grid-cols-2">
        <article class="rounded-2xl border p-6 sm:p-8" :class="surfaceClass">
          <div
            class="mb-5 flex h-12 w-12 items-center justify-center rounded-xl bg-violet-500/15 text-2xl"
          >
            🎯
          </div>

          <h2 class="text-xl font-bold" :class="textClass">
            Projenin amacı
          </h2>

          <p class="mt-4 text-sm leading-7" :class="mutedClass">
            TaskFlow'un amacı, günlük işlerini tek bir yerde düzenleyebilmeni
            sağlamaktır. Görevleri kategorilere ayırabilir, önceliklerini
            belirleyebilir, son tarihlerini takip edebilir ve tamamladığın
            işleri kolayca görüntüleyebilirsin.
          </p>

          <p class="mt-4 text-sm leading-7" :class="mutedClass">
            Uygulama, görevlerini yönetmeni kolaylaştırırken ilerlemeni
            istatistikler üzerinden takip etmene de yardımcı olur.
          </p>
        </article>

        <article class="rounded-2xl border p-6 sm:p-8" :class="surfaceClass">
          <div
            class="mb-5 flex h-12 w-12 items-center justify-center rounded-xl bg-emerald-500/15 text-2xl"
          >
            ✨
          </div>

          <h2 class="text-xl font-bold" :class="textClass">
            Tasarım yaklaşımı
          </h2>

          <p class="mt-4 text-sm leading-7" :class="mutedClass">
            Sade bir arayüz, kolay anlaşılır kontroller ve tutarlı renkler
            kullanarak görev yönetimini mümkün olduğunca pratik hâle getirmeyi
            hedefliyoruz.
          </p>

          <p class="mt-4 text-sm leading-7" :class="mutedClass">
            Koyu ve açık tema seçenekleri, farklı ekran boyutlarına uyumlu
            yerleşim ve anlık güncellenen istatistikler bu yaklaşımın parçalarıdır.
          </p>
        </article>
      </section>

      <!-- Özellikler -->
      <section class="mb-8">
        <div class="mb-6">
          <p class="mb-2 text-xs font-semibold uppercase tracking-[0.2em] text-violet-400">
            Neler yapabilirsin?
          </p>

          <h2 class="text-2xl font-bold sm:text-3xl" :class="textClass">
            İhtiyacın olan araçlar.
          </h2>

          <p class="mt-2 text-sm" :class="mutedClass">
            Günlük işlerini düzenlemek için tasarlanmış özellikler.
          </p>
        </div>

        <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-3">
          <article
            v-for="feature in features"
            :key="feature.title"
            class="feature-card rounded-2xl border p-5 sm:p-6"
            :class="surfaceClass"
          >
            <div
              class="mb-5 flex h-11 w-11 items-center justify-center rounded-xl text-xl"
              :class="feature.iconClass"
            >
              {{ feature.icon }}
            </div>

            <h3 class="font-bold" :class="textClass">
              {{ feature.title }}
            </h3>

            <p class="mt-2 text-sm leading-6" :class="mutedClass">
              {{ feature.description }}
            </p>
          </article>
        </div>
      </section>

      <!-- Teknolojiler -->
      <section class="mb-8">
        <div class="mb-6">
          <p class="mb-2 text-xs font-semibold uppercase tracking-[0.2em] text-violet-400">
            Teknoloji altyapısı
          </p>

          <h2 class="text-2xl font-bold sm:text-3xl" :class="textClass">
            Kullanılan teknolojiler.
          </h2>

          <p class="mt-2 text-sm" :class="mutedClass">
            Uygulamanın arayüzü, durumu ve sayfa yapısı için kullanılan araçlar.
          </p>
        </div>

        <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
          <article
            v-for="technology in technologies"
            :key="technology.name"
            class="flex items-start gap-4 rounded-2xl border p-5"
            :class="surfaceClass"
          >
            <div
              class="flex h-12 w-12 shrink-0 items-center justify-center rounded-xl text-lg font-bold"
              :class="technology.iconClass"
            >
              {{ technology.icon }}
            </div>

            <div>
              <h3 class="font-bold" :class="textClass">
                {{ technology.name }}
              </h3>

              <p class="mt-1 text-sm leading-6" :class="mutedClass">
                {{ technology.description }}
              </p>
            </div>
          </article>
        </div>
      </section>

      <!-- Proje bilgileri -->
      <section
        class="rounded-2xl border p-6 sm:p-8"
        :class="surfaceClass"
      >
        <div class="grid gap-8 md:grid-cols-2">
          <div>
            <h2 class="text-lg font-bold" :class="textClass">
              Proje bilgileri
            </h2>

            <p class="mt-2 text-sm leading-6" :class="mutedClass">
              TaskFlow, modern web uygulaması geliştirme pratiklerini uygulamak
              amacıyla oluşturulmuş bir görev yönetimi projesidir.
            </p>
          </div>

          <div class="space-y-4">
            <div
              class="flex items-center justify-between gap-4 border-b pb-3"
              :class="dividerClass"
            >
              <span class="text-sm" :class="mutedClass">
                Uygulama adı
              </span>

              <span class="text-sm font-semibold" :class="textClass">
                TaskFlow
              </span>
            </div>

            <div
              class="flex items-center justify-between gap-4 border-b pb-3"
              :class="dividerClass"
            >
              <span class="text-sm" :class="mutedClass">
                Uygulama türü
              </span>

              <span class="text-sm font-semibold" :class="textClass">
                Görev yönetimi
              </span>
            </div>

            <div
              class="flex items-center justify-between gap-4 border-b pb-3"
              :class="dividerClass"
            >
              <span class="text-sm" :class="mutedClass">
                Arayüz
              </span>

              <span class="text-sm font-semibold" :class="textClass">
                Vue 3
              </span>
            </div>

            <div class="flex items-center justify-between gap-4">
              <span class="text-sm" :class="mutedClass">
                Veri saklama
              </span>

              <span class="text-sm font-semibold" :class="textClass">
                LocalStorage
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- Son çağrı -->
      <section class="py-10 text-center sm:py-14">
        <div class="mb-4 text-3xl">🚀</div>

        <h2 class="text-2xl font-bold sm:text-3xl" :class="textClass">
          Planlamaya hazır mısın?
        </h2>

        <p class="mx-auto mt-3 max-w-lg text-sm leading-6" :class="mutedClass">
          İlk görevini oluştur, önceliklerini belirle ve ilerlemeni takip etmeye başla.
        </p>

        <RouterLink
          to="/"
          class="mt-6 inline-flex items-center gap-2 rounded-xl bg-violet-600 px-6 py-3 text-sm font-semibold text-white transition-all hover:-translate-y-0.5 hover:bg-violet-500"
        >
          TaskFlow'u kullan
          <span>→</span>
        </RouterLink>
      </section>
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

const dividerClass = computed(() =>
  isLight.value ? 'border-gray-100' : 'border-white/[0.07]',
)

const metrics = computed(() => [
  {
    label: 'Toplam görev',
    value: store.todos.length,
    description: 'Oluşturulan görevlar',
    color: textClass.value,
  },
  {
    label: 'Tamamlanan',
    value: store.completedCount,
    description: 'Bitirilen görevler',
    color: 'text-emerald-400',
  },
  {
    label: 'Devam eden',
    value: store.remainingCount,
    description: 'Aktif görevler',
    color: 'text-blue-400',
  },
  {
    label: 'Geciken',
    value: store.overdueCount,
    description: 'Son tarihi geçmiş',
    color: 'text-red-400',
  },
])

const features = [
  {
    icon: '✓',
    iconClass: 'bg-emerald-500/15 text-emerald-400',
    title: 'Görev yönetimi',
    description:
      'Görev oluştur, düzenle, tamamlandı olarak işaretle veya artık ihtiyacın olmayanları sil.',
  },
  {
    icon: '🎯',
    iconClass: 'bg-red-500/15 text-red-400',
    title: 'Öncelik belirleme',
    description:
      'Görevlerini yüksek, orta ve düşük öncelik seviyelerine göre düzenle.',
  },
  {
    icon: '📅',
    iconClass: 'bg-blue-500/15 text-blue-400',
    title: 'Son tarih takibi',
    description:
      'Görevlerine son tarih ekle ve süresi geçmiş görevlerini kolayca belirle.',
  },
  {
    icon: '⌕',
    iconClass: 'bg-violet-500/15 text-violet-400',
    title: 'Arama ve filtreleme',
    description:
      'Görevlerini başlığa, tamamlanma durumuna veya kategoriye göre filtrele.',
  },
  {
    icon: '▥',
    iconClass: 'bg-amber-500/15 text-amber-400',
    title: 'İstatistikler',
    description:
      'Tamamlanma oranını, görev dağılımını ve kategori performansını görüntüle.',
  },
  {
    icon: '◐',
    iconClass: 'bg-pink-500/15 text-pink-400',
    title: 'Tema seçenekleri',
    description:
      'Uygulamayı tercihlerine göre koyu veya açık temada kullan.',
  },
  {
    icon: '↕',
    iconClass: 'bg-cyan-500/15 text-cyan-400',
    title: 'Sürükle ve bırak',
    description:
      'Görevlerini sürükleyerek listendeki sıralamayı değiştirebilirsin.',
  },
  {
    icon: '💾',
    iconClass: 'bg-indigo-500/15 text-indigo-400',
    title: 'Yerel veri saklama',
    description:
      'Görevlerin tarayıcının LocalStorage alanında saklanır ve sayfayı yenilediğinde korunur.',
  },
  {
    icon: '📱',
    iconClass: 'bg-orange-500/15 text-orange-400',
    title: 'Duyarlı tasarım',
    description:
      'Farklı ekran boyutlarına uyum sağlayan bir arayüzle görevlerini yönetebilirsin.',
  },
]

const technologies = [
  {
    icon: 'V',
    iconClass: 'bg-emerald-500/10 text-emerald-400',
    name: 'Vue 3',
    description:
      'Bileşen tabanlı yapı ve Composition API ile arayüz geliştirme.',
  },
  {
    icon: 'P',
    iconClass: 'bg-amber-500/10 text-amber-400',
    name: 'Pinia',
    description:
      'Görevlerin ve tema tercihlerinin merkezi durum yönetimi.',
  },
  {
    icon: 'TW',
    iconClass: 'bg-cyan-500/10 text-cyan-400',
    name: 'Tailwind CSS',
    description:
      'Modern, tutarlı ve duyarlı arayüz stilleri.',
  },
  {
    icon: 'R',
    iconClass: 'bg-violet-500/10 text-violet-400',
    name: 'Vue Router',
    description:
      'Görevler, istatistikler ve hakkında sayfaları arasında gezinme.',
  },
  {
    icon: '⚡',
    iconClass: 'bg-yellow-500/10 text-yellow-400',
    name: 'Vite',
    description:
      'Hızlı geliştirme sunucusu ve üretim derlemesi.',
  },
  {
    icon: 'JS',
    iconClass: 'bg-blue-500/10 text-blue-400',
    name: 'JavaScript',
    description:
      'Görev işlemleri, filtreler ve hesaplamalar için uygulama mantığı.',
  },
]
</script>

<style scoped>
.about-page {
  transition:
    background-color 0.25s ease,
    color 0.25s ease;
}

.hero-panel {
  isolation: isolate;
}

.logo-orbit {
  animation: orbit-float 6s ease-in-out infinite;
}

.feature-card {
  transition:
    transform 0.2s ease,
    border-color 0.2s ease,
    box-shadow 0.2s ease;
}

.feature-card:hover {
  transform: translateY(-3px);
}

.about-dark .feature-card:hover {
  border-color: rgba(167, 139, 250, 0.22);
}

.about-light .feature-card:hover {
  border-color: rgba(139, 92, 246, 0.25);
  box-shadow: 0 12px 30px rgba(31, 25, 70, 0.05);
}

@keyframes orbit-float {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-6px);
  }
}

@media (prefers-reduced-motion: reduce) {
  .logo-orbit {
    animation: none;
  }

  .feature-card {
    transition: none;
  }

  .feature-card:hover {
    transform: none;
  }
}
</style>
```
