<template>
  <section class="relative overflow-hidden min-h-screen bg-gradient-to-b from-[#fce5ea] via-white to-white">
    <div class="absolute inset-0 pointer-events-none">
      <div class="absolute -top-32 right-10 w-80 h-80 rounded-full bg-rose-200/50 blur-[140px]"></div>
      <div class="absolute bottom-0 left-0 w-[32rem] h-[32rem] rounded-full bg-rose-100/70 blur-[200px]"></div>
      <div class="absolute top-1/3 -left-10 w-40 h-40 rounded-full bg-white/70 blur-[120px]"></div>
    </div>

    <div class="relative max-w-6xl mx-auto px-5 py-20 space-y-12">
      <header class="text-center space-y-4">
        <p class="text-xs font-semibold tracking-[0.35em] text-rose-500 uppercase">ivy surrogacy</p>
        <h1 class="text-3xl md:text-4xl font-bold text-[#3d3d3d]">
          Find Surrogates That Match Your Journey
        </h1>
        <p class="text-gray-600 max-w-3xl mx-auto leading-relaxed">
          Browse a curated roster of surrogates with transparent details and intuitive filters. Compare profiles,
          refine criteria, and connect with confidence.
        </p>
        <div class="flex flex-wrap justify-center gap-2 text-[11px] uppercase tracking-wide text-rose-500">
          <span class="chip">Verified profiles</span>
          <span class="chip">Daily updates</span>
          <span class="chip">100% human reviewed</span>
        </div>
      </header>

      <div class="glass-panel shadow-rose-200/60">
        <form class="space-y-6" @submit.prevent="handleSearch">
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-6 gap-6 items-end">
            <div class="field">
              <label>Age</label>
              <div class="flex items-center gap-2">
                <select v-model="filters.age" class="input">
                  <option value="">Any</option>
                  <option v-for="option in ageOptions" :key="option" :value="option">
                    {{ option }}
                  </option>
                </select>
                <span class="text-xs text-gray-500">or younger</span>
              </div>
            </div>

            <div class="field">
              <label>State</label>
              <select v-model="filters.state" class="input">
                <option value="">Nothing selected</option>
                <option v-for="state in stateOptions" :key="state" :value="state">
                  {{ state }}
                </option>
              </select>
            </div>

            <div class="field">
              <label>Race</label>
              <select v-model="filters.race" class="input">
                <option value="">Nothing selected</option>
                <option v-for="race in raceOptions" :key="race" :value="race">
                  {{ race }}
                </option>
              </select>
            </div>

            <div class="field">
              <label>Experienced</label>
              <select v-model="filters.experience" class="input">
                <option value="">Any</option>
                <option value="yes">Yes</option>
                <option value="no">No</option>
              </select>
            </div>

            <div class="field">
              <label>Keyword</label>
              <input v-model="filters.keyword" type="text" placeholder="Surrogate ID, City..." class="input" />
            </div>

            <button
              type="submit"
              class="btn-primary w-full flex items-center justify-center h-[46px]"
              aria-label="Search"
            >
              Search
            </button>
          </div>
        </form>
      </div>

      <div class="space-y-8">
        <div class="glass-panel flex flex-wrap items-center justify-between gap-3 px-6 py-4 shadow-md">
          <p class="text-sm text-gray-600">
            Showing
            <span class="font-semibold text-rose-500">{{ paginatedSurrogates.length }}</span>
            of
            <span class="font-semibold text-rose-500">{{ filteredSurrogates.length }}</span>
            matching surrogates
          </p>
          <p class="text-xs uppercase tracking-wide text-gray-400">UPDATED DAILY</p>
        </div>

        <ClientOnly v-if="paginatedSurrogates.length">
          <TransitionGroup name="card-list" tag="div" class="space-y-8">
            <article
              v-for="(surrogate, index) in paginatedSurrogates"
              :key="surrogate.id"
              v-motion
              :initial="{ opacity: 0, y: 30 }"
              :enter="{ opacity: 1, y: 0, transition: { duration: 400, delay: index * 60 } }"
              class="card group items-center shadow-lg"
            >
              <div class="avatar-placeholder group-hover:shadow-rose-200/60 transition-shadow">?</div>

              <div class="flex-1 grid gap-6 md:grid-cols-3 text-[15px] leading-relaxed text-gray-600">
                <div class="space-y-2">
                  <p>
                    <span class="font-semibold text-rose-500">Surrogate ID:</span>
                    <button class="text-rose-500 underline-offset-2 hover:underline font-semibold">
                      {{ surrogate.id }}
                    </button>
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Marital Status:</span>
                    <span class="text-rose-500">{{ surrogate.maritalStatus }}</span>
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Race:</span>
                    <span class="text-rose-500">{{ surrogate.race }}</span>
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Number of C-Section:</span>
                    {{ surrogate.cSections }}
                  </p>
                </div>

                <div class="space-y-2">
                  <p>
                    <span class="font-semibold text-gray-700">Age:</span>
                    {{ surrogate.age }}
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Employed:</span>
                    <span class="text-rose-500">{{ formatBoolean(surrogate.employed) }}</span>
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">BMI:</span>
                    {{ surrogate.bmi }}
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Last Delivery:</span>
                    {{ surrogate.lastDelivery }}
                  </p>
                </div>

                <div class="space-y-2">
                  <p>
                    <span class="font-semibold text-gray-700">Location City, State:</span>
                    <span class="text-rose-500">
                      {{ surrogate.locationCity }}, {{ surrogate.locationState }}
                    </span>
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Experienced:</span>
                    <span class="text-rose-500">{{ formatBoolean(surrogate.experienced) }}</span>
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Own Children:</span>
                    {{ surrogate.ownChildren }}
                  </p>
                  <p>
                    <span class="font-semibold text-gray-700">Total Deliveries:</span>
                    {{ surrogate.totalDeliveries }}
                  </p>
                </div>
              </div>
            </article>
          </TransitionGroup>
        </ClientOnly>

        <div v-else class="glass-panel text-center py-12 border border-dashed border-rose-200/70 shadow-md">
          <p class="text-rose-500 font-semibold text-lg">No surrogates match the selected filters.</p>
          <p class="text-sm text-gray-500 mt-2">Try adjusting search criteria.</p>
        </div>
      </div>

      <div v-if="totalPages > 1" class="flex justify-center items-center gap-4 pt-6">
        <button class="pagination-btn" :disabled="currentPage === 1" @click="goToPage(currentPage - 1)">
          «
        </button>

        <button
          v-for="pageNumber in pageNumbers"
          :key="pageNumber"
          class="pagination-btn"
          :class="{ 'pagination-btn-active': currentPage === pageNumber }"
          @click="goToPage(pageNumber)"
        >
          {{ pageNumber }}
        </button>

        <button class="pagination-btn" :disabled="currentPage === totalPages" @click="goToPage(currentPage + 1)">
          »
        </button>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { computed, reactive, ref, watch } from 'vue'

type Surrogate = {
  id: string
  age: number
  maritalStatus: string
  race: string
  cSections: number
  locationState: string
  locationCity: string
  employed: boolean
  experienced: boolean
  bmi: number
  lastDelivery: string
  ownChildren: number
  totalDeliveries: number
  state: string
}

const rawSurrogates = ref<Surrogate[]>([
  {
    id: 'G0509',
    age: 27,
    maritalStatus: 'Single',
    race: 'White/Caucasian',
    cSections: 0,
    locationState: 'Texas',
    locationCity: 'Corsicana',
    employed: true,
    experienced: false,
    bmi: 20,
    lastDelivery: '2025-06-03',
    ownChildren: 3,
    totalDeliveries: 3,
    state: 'TX'
  },
  {
    id: 'G0508',
    age: 27,
    maritalStatus: 'Single',
    race: 'African descent/Black',
    cSections: 1,
    locationState: 'California',
    locationCity: 'Yuba City',
    employed: true,
    experienced: false,
    bmi: 21,
    lastDelivery: '2020-12-14',
    ownChildren: 1,
    totalDeliveries: 1,
    state: 'CA'
  },
  {
    id: 'G0506',
    age: 36,
    maritalStatus: 'Single',
    race: 'Latino/Hispanic',
    cSections: 0,
    locationState: 'California',
    locationCity: 'Rancho Cucamonga',
    employed: false,
    experienced: false,
    bmi: 28,
    lastDelivery: '2024-07-26',
    ownChildren: 4,
    totalDeliveries: 4,
    state: 'CA'
  },
  {
    id: 'G0505',
    age: 33,
    maritalStatus: 'Separated',
    race: 'Asian',
    cSections: 0,
    locationState: 'California',
    locationCity: 'Perris',
    employed: true,
    experienced: true,
    bmi: 23,
    lastDelivery: '2023-08-10',
    ownChildren: 2,
    totalDeliveries: 2,
    state: 'CA'
  },
  {
    id: 'G0504',
    age: 29,
    maritalStatus: 'Married',
    race: 'White/Caucasian',
    cSections: 0,
    locationState: 'Nevada',
    locationCity: 'Las Vegas',
    employed: true,
    experienced: true,
    bmi: 22,
    lastDelivery: '2022-03-12',
    ownChildren: 2,
    totalDeliveries: 2,
    state: 'NV'
  },
  {
    id: 'G0503',
    age: 31,
    maritalStatus: 'Married',
    race: 'African descent/Black',
    cSections: 2,
    locationState: 'Arizona',
    locationCity: 'Phoenix',
    employed: false,
    experienced: true,
    bmi: 24,
    lastDelivery: '2021-11-02',
    ownChildren: 3,
    totalDeliveries: 3,
    state: 'AZ'
  },
  {
    id: 'G0502',
    age: 24,
    maritalStatus: 'Single',
    race: 'Latino/Hispanic',
    cSections: 0,
    locationState: 'New Mexico',
    locationCity: 'Santa Fe',
    employed: true,
    experienced: false,
    bmi: 19,
    lastDelivery: '2024-04-18',
    ownChildren: 1,
    totalDeliveries: 1,
    state: 'NM'
  },
  {
    id: 'G0501',
    age: 34,
    maritalStatus: 'Married',
    race: 'Asian',
    cSections: 1,
    locationState: 'Oregon',
    locationCity: 'Portland',
    employed: true,
    experienced: true,
    bmi: 25,
    lastDelivery: '2023-01-09',
    ownChildren: 2,
    totalDeliveries: 3,
    state: 'OR'
  }
])

const initialFilters = {
  age: '',
  state: '',
  race: '',
  experience: '',
  keyword: ''
}

const filters = reactive({ ...initialFilters })

const ageOptions = [25, 30, 35, 40, 45]
const stateOptions = ['California', 'Texas', 'Nevada', 'Arizona', 'New Mexico', 'Oregon']
const raceOptions = ['White/Caucasian', 'African descent/Black', 'Latino/Hispanic', 'Asian']

const currentPage = ref(1)
const perPage = 4

const filteredSurrogates = computed(() => {
  return rawSurrogates.value.filter((surrogate) => {
    const ageLimit = filters.age ? Number(filters.age) : null
    if (ageLimit && surrogate.age > ageLimit) {
      return false
    }

    if (filters.state && surrogate.locationState !== filters.state) {
      return false
    }

    if (filters.race && surrogate.race !== filters.race) {
      return false
    }

    if (filters.experience) {
      const wantsExperienced = filters.experience === 'yes'
      if (surrogate.experienced !== wantsExperienced) {
        return false
      }
    }

    if (filters.keyword) {
      const keyword = filters.keyword.toLowerCase()
      const text = `${surrogate.id} ${surrogate.locationCity} ${surrogate.locationState}`.toLowerCase()
      if (!text.includes(keyword)) {
        return false
      }
    }

    return true
  })
})

const totalPages = computed(() => Math.max(1, Math.ceil(filteredSurrogates.value.length / perPage)))

const paginatedSurrogates = computed(() => {
  const start = (currentPage.value - 1) * perPage
  return filteredSurrogates.value.slice(start, start + perPage)
})

const pageNumbers = computed(() => {
  return Array.from({ length: totalPages.value }, (_, index) => index + 1)
})

watch(
  filters,
  () => {
    currentPage.value = 1
  },
  { deep: true }
)

const goToPage = (page: number) => {
  if (page < 1 || page > totalPages.value) {
    return
  }
  currentPage.value = page
}

const handleSearch = () => {
  currentPage.value = 1
}

const formatBoolean = (value: boolean) => (value ? 'Yes' : 'No')
</script>

<style scoped>
.glass-panel {
  @apply rounded-[32px] border border-pink-100 bg-white/85 backdrop-blur-md shadow-[0_20px_45px_rgba(201,58,103,0.08)] p-6 md:p-8;
}

.field {
  @apply flex flex-col gap-2;
}

.field label {
  @apply text-xs font-semibold tracking-wide text-gray-500 uppercase;
}

.input {
  @apply w-full rounded-2xl border border-pink-100 bg-white px-4 py-2.5 text-sm text-gray-700 placeholder:text-gray-400 shadow-inner focus:border-pink-200 focus:ring-2 focus:ring-pink-100 transition duration-300 ease-in-out;
}

.btn-primary {
  @apply inline-flex items-center justify-center px-6 py-3 rounded-2xl font-semibold text-white bg-pink-500 shadow-lg shadow-pink-200/60 hover:bg-pink-600 focus:outline-none focus:ring-2 focus:ring-pink-200 transition duration-300;
}

.chip {
  @apply rounded-full bg-white/60 px-3 py-1 border border-rose-100 text-rose-500 shadow-sm;
}

.card {
  @apply glass-panel p-6 flex flex-col md:flex-row md:items-center gap-6 border border-pink-100 hover:border-pink-200 transition duration-300 shadow-[0_12px_35px_rgba(201,58,103,0.08)];
}

.avatar-placeholder {
  @apply w-28 h-28 rounded-2xl bg-gradient-to-b from-gray-100 to-gray-200 border border-white/70 flex items-center justify-center text-4xl text-gray-400 shadow-inner transition duration-300;
}

.pagination-btn {
  @apply w-10 h-10 rounded-full border border-pink-100 bg-white text-pink-500 font-semibold shadow-[0_6px_18px_rgba(201,58,103,0.18)] flex items-center justify-center hover:bg-pink-500 hover:text-white transition duration-300 disabled:opacity-40 disabled:hover:bg-white disabled:hover:text-pink-400;
}

.pagination-btn-active {
  @apply bg-pink-500 text-white shadow-[0_10px_24px_rgba(201,58,103,0.25)] border-pink-500;
}

.card-list-enter-active,
.card-list-leave-active {
  transition: all 0.4s ease;
}

.card-list-enter-from,
.card-list-leave-to {
  opacity: 0;
  transform: translateY(12px);
}
</style>
