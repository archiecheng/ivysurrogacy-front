<template>
  <div class="min-h-screen bg-gradient-to-b from-[#ffe6eb] to-white flex flex-col">
    <div class="px-6 py-4 flex items-center justify-between text-sm font-semibold text-gray-500">
      <NuxtLink to="/" class="flex items-center gap-1 hover:text-pink-500 transition">
        <span class="text-lg">←</span>
        Back
      </NuxtLink>
      <NuxtLink to="/signup" class="hover:text-pink-500 transition">Create Account</NuxtLink>
    </div>

    <section class="flex-1 flex items-center justify-center px-4 py-10">
      <div
        v-motion
        :initial="{ opacity: 0, y: 20 }"
        :enter="{ opacity: 1, y: 0, transition: { duration: 400 } }"
        class="w-full max-w-5xl bg-white/95 backdrop-blur rounded-[32px] shadow-[0_25px_80px_rgba(249,116,154,0.18)] grid gap-6 md:grid-cols-[1.1fr_0.9fr] overflow-hidden"
      >
        <div class="px-10 py-12 flex flex-col gap-8">
          <div>
            <p class="text-xs uppercase tracking-[0.35em] text-pink-400">welcome back</p>
            <h1 class="text-3xl font-semibold text-gray-800 mt-1">Log in to Ivy Surrogacy</h1>
            <p class="text-sm text-gray-500 mt-2">
              Enter your credentials to access personalized updates, saved profiles, and applications.
            </p>
          </div>

          <form class="space-y-6" @submit.prevent="handleLogin">
            <div class="space-y-2">
              <label class="text-sm font-medium text-gray-600" for="email">Email address</label>
              <input
                id="email"
                v-model="form.email"
                type="email"
                placeholder="you@example.com"
                required
                class="w-full border-b border-gray-300 bg-transparent px-0 py-2 text-gray-800 placeholder:text-gray-400 focus:border-pink-400 focus:outline-none"
              />
            </div>

            <div class="space-y-2">
              <label class="text-sm font-medium text-gray-600" for="password">Password</label>
              <div class="flex items-center border-b border-gray-300 focus-within:border-pink-400 transition">
                <input
                  id="password"
                  v-model="form.password"
                  type="password"
                  placeholder="Enter your password"
                  required
                  class="flex-1 bg-transparent px-0 py-2 text-gray-800 placeholder:text-gray-400 focus:outline-none"
                />
                <button type="button" class="text-xs text-gray-400 hover:text-pink-500 transition">
                  Show
                </button>
              </div>
            </div>

            <button
              type="submit"
              class="w-full rounded-full bg-pink-500 py-3 text-white font-semibold shadow-sm hover:bg-pink-600 hover:shadow-md focus:outline-none focus:ring-2 focus:ring-pink-200 transition duration-200 ease-in-out active:scale-95"
            >
              Log In
            </button>
          </form>

          <div class="flex flex-wrap items-center gap-4 text-sm text-gray-500">
            <NuxtLink to="/signup" class="text-pink-500 hover:text-pink-600 transition">Create account</NuxtLink>
            <NuxtLink to="/forgot-password" class="hover:text-pink-600 transition">Forgot password?</NuxtLink>
          </div>
        </div>

        <div class="border-t md:border-t-0 md:border-l border-pink-50 px-10 py-12 flex flex-col gap-4 bg-white/60">
          <p class="text-xs uppercase tracking-[0.4em] text-gray-400">or</p>
          <h3 class="text-lg font-semibold text-gray-800">Continue with</h3>

          <div class="space-y-3">
            <button v-for="provider in providers" :key="provider.label" class="provider-btn">
              <span class="w-6 h-6 rounded-full bg-pink-50 text-pink-500 flex items-center justify-center text-sm font-semibold">
                {{ provider.short }}
              </span>
              {{ provider.label }}
            </button>
          </div>

          <p class="text-xs text-gray-400 mt-6">
            Secure login powered by Ivy Surrogacy. By continuing you agree to our
            <NuxtLink to="/terms" class="text-pink-500 hover:text-pink-600">Terms & Privacy</NuxtLink>.
          </p>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">

definePageMeta({
  layout: false
})

const form = reactive({
  email: '',
  password: ''
})

const providers = [
  { label: 'Continue with Google', short: 'G' },
  { label: 'Continue with Apple', short: '' },
  { label: 'Continue with Facebook', short: 'f' }
]

const handleLogin = () => {
  console.log('login', form)
}
</script>

<style scoped>
.provider-btn {
  @apply w-full flex items-center gap-3 border border-gray-200 rounded-2xl px-4 py-2.5 text-sm font-medium text-gray-700 hover:border-pink-300 hover:text-pink-500 transition;
}
</style>
