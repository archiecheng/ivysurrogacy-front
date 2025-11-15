<template>
  <div class="min-h-screen bg-gradient-to-b from-[#ffe6eb] to-white flex flex-col">
    <div class="px-6 py-4 flex items-center justify-between text-sm font-semibold text-gray-500">
      <NuxtLink to="/" class="flex items-center gap-1 hover:text-pink-500 transition">
        <span class="text-lg">←</span>
        Back
      </NuxtLink>
      <NuxtLink to="/login" class="hover:text-pink-500 transition">Log In</NuxtLink>
    </div>

    <section class="flex-1 flex items-center justify-center px-4 py-8 box-border">
      <div
        v-motion
        :initial="{ opacity: 0, y: 24 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 350 } }"
        class="w-full max-w-3xl bg-white/95 backdrop-blur rounded-[32px] shadow-[0_25px_75px_rgba(249,116,154,0.18)]"
      >
        <div class="px-10 py-12 flex flex-col">
          <div>
            <p class="text-xs uppercase tracking-[0.35em] text-pink-400">create</p>
            <h1 class="text-3xl font-semibold text-gray-800 mt-1">Sign up for Ivy Surrogacy</h1>
            <p class="text-sm text-gray-500 mt-2">
              Create your account to save profiles, track applications, and stay connected with Ivy coordinators.
            </p>
          </div>

          <form class="space-y-6 mb-4" @submit.prevent="handleSignup">
            <div class="space-y-2">
              <label class="text-sm font-medium text-gray-600" for="email">Email address</label>
              <input id="email" v-model="form.email" type="email" placeholder="you@example.com" required class="underlined-input" />
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-gray-600" for="password">Password</label>
              <input
                id="password"
                v-model="form.password"
                type="password"
                placeholder="Create a secure password"
                required
                class="underlined-input"
              />
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-gray-600" for="confirmPassword">Confirm password</label>
              <input
                id="confirmPassword"
                v-model="form.confirmPassword"
                type="password"
                placeholder="Confirm your password"
                required
                class="underlined-input"
              />
            </div>

            <div class="space-y-3">
              <p class="text-xs uppercase tracking-[0.3em] text-gray-500">I'm signing up as</p>
              <div class="flex flex-wrap gap-2">
                <button
                  v-for="option in roleOptions"
                  :key="option.value"
                  type="button"
                  class="role-chip"
                  :class="{ 'bg-pink-500 text-white border-transparent shadow-sm': form.role === option.value }"
                  @click="form.role = option.value"
                >
                  {{ option.label }}
                </button>
              </div>
            </div>

            <button
              type="submit"
              class="w-full rounded-full bg-pink-500 py-3 text-white font-semibold shadow-sm hover:bg-pink-600 hover:shadow-md focus:outline-none focus:ring-2 focus:ring-pink-200 transition duration-200 ease-in-out active:scale-95"
            >
              Sign Up
            </button>
          </form>

          <div class="flex flex-wrap items-center gap-4 text-sm text-gray-500 justify-center">
            <NuxtLink to="/login" class="text-pink-500 hover:text-pink-600 transition">Already have an account?</NuxtLink>
            <NuxtLink to="/forgot-password" class="hover:text-pink-600 transition">Forgot password?</NuxtLink>
          </div>
        </div>

      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  layout: false
})

const roleOptions = [
  { label: 'Surrogate Mother', value: 'surrogate' },
  { label: 'Egg Donor', value: 'donor' },
  { label: 'Future Parent', value: 'parent' }
]

const form = reactive({
  email: '',
  password: '',
  confirmPassword: '',
  role: 'surrogate'
})

const handleSignup = () => {
  console.log('signup', form)
}
</script>

<style scoped>
.underlined-input {
  @apply w-full border-b border-gray-300 bg-transparent px-0 py-2 text-gray-800 placeholder:text-gray-400 focus:border-pink-400 focus:outline-none transition;
}

.role-chip {
  @apply px-4 py-2 rounded-full border border-gray-300 text-sm text-gray-600 bg-white hover:border-pink-400 hover:text-pink-500 transition;
}

</style>
