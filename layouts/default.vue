<template>
  <div class="min-h-screen flex flex-col bg-white text-slate-800">
    <header class="shadow-sm">
      <div class="bg-rose-400 text-white text-sm">
        <div class="max-w-6xl mx-auto px-4 py-2 flex flex-col gap-2 md:flex-row md:items-center md:justify-between">
          <div class="flex flex-wrap items-center gap-x-6 gap-y-2">
            <span class="flex items-center gap-2">
              <PhoneIcon class="h-4 w-4" />
              <span>(626)545-1617</span>
            </span>
            <span class="flex items-center gap-2">
              <EnvelopeIcon class="h-4 w-4" />
              <span>contact@ivysurrogacy.com</span>
            </span>
          </div>
          <div class="flex items-center gap-4 text-base">
            <div class="flex items-center gap-3">
              <a
                v-for="social in socials"
                :key="social.name"
                :href="social.href"
                class="hover:text-white/80 transition"
                :aria-label="social.name"
              >
                {{ social.label }}
              </a>
            </div>
            <select class="bg-white/85 text-gray-900 text-sm rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-white/70">
              <option>English</option>
              <option>中文</option>
            </select>
          </div>
        </div>
      </div>

      <div class="bg-white">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col gap-4 md:flex-row md:items-center md:justify-between">
          <div class="flex items-center gap-3">
            <div class="w-12 h-12 rounded-full bg-rose-400 text-white flex items-center justify-center font-semibold text-lg">
              IVY
            </div>
            <div>
              <p class="text-lg font-semibold text-rose-500">IVY Surrogacy</p>
              <p class="text-xs text-slate-500">Building families with care</p>
            </div>
          </div>

          <nav class="flex-1">
            <ul class="flex flex-wrap items-center gap-x-6 gap-y-2 text-sm font-medium text-slate-700">
              <li v-for="link in navLinks" :key="link.label" class="flex items-center gap-1 cursor-pointer hover:text-rose-500 transition">
                <a :href="link.href">{{ link.label }}</a>
                <ChevronDownIcon v-if="link.dropdown" class="h-4 w-4" />
              </li>
            </ul>
          </nav>

          <NuxtLink
            to="/login"
            class="hidden md:inline-flex items-center gap-2 text-sm font-semibold text-rose-500 border border-rose-200 rounded-full px-4 py-1.5 hover:bg-rose-50 transition"
          >
            Login / Signup
          </NuxtLink>
        </div>
      </div>
    </header>

    <main class="flex-1">
      <slot></slot>
    </main>

    <footer class="bg-rose-400 text-white mt-12">
      <div class="max-w-6xl mx-auto px-4 py-12 grid gap-10 md:grid-cols-4">
        <div class="space-y-4">
          <div class="flex items-center gap-3">
            <div class="w-12 h-12 rounded-full bg-white/30 text-white flex items-center justify-center font-semibold text-lg">
              IVY
            </div>
            <div>
              <p class="text-xl font-semibold">IVY Surrogacy</p>
              <p class="text-sm text-white/80">Growing families since 2015</p>
            </div>
          </div>
          <ul class="space-y-2 text-sm text-white/90">
            <li class="flex items-center gap-2">
              <MapPinIcon class="h-4 w-4" />
              <span>680 Brea Canyon Rd, Unit 288<br />Diamond Bar, CA 91789</span>
            </li>
            <li class="flex items-center gap-2">
              <EnvelopeIcon class="h-4 w-4" />
              <span>contact@ivysurrogacy.com</span>
            </li>
            <li class="flex items-center gap-2">
              <PhoneIcon class="h-4 w-4" />
              <span>(626)545-1617</span>
            </li>
          </ul>
        </div>

        <div v-for="column in footerColumns" :key="column.title" class="space-y-3">
          <p class="font-semibold text-lg">{{ column.title }}</p>
          <ul class="space-y-2 text-sm text-white/90">
            <li v-for="item in column.links" :key="item" class="hover:text-white transition cursor-pointer">
              {{ item }}
            </li>
          </ul>
        </div>
      </div>

      <div class="border-t border-white/20">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-col gap-2 md:flex-row md:items-center md:justify-between text-sm text-white/80">
          <p>© {{ currentYear }} Ivy Surrogacy. All rights reserved.</p>
          <div class="flex flex-wrap items-center gap-4">
            <a v-for="policy in policies" :key="policy" href="#" class="hover:text-white transition">
              {{ policy }}
            </a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { EnvelopeIcon, PhoneIcon, MapPinIcon, ChevronDownIcon, MagnifyingGlassIcon } from '@heroicons/vue/24/outline'

const navLinks = [
  { label: 'Intended Parents', href: '#', dropdown: true },
  { label: 'Surrogates', href: '#', dropdown: true },
  { label: 'Egg Donors', href: '#', dropdown: true },
  { label: 'Resources', href: '#', dropdown: true },
  { label: 'About', href: '#', dropdown: false },
  { label: 'Contact', href: '#', dropdown: false }
]

const socials = [
  { name: 'Facebook', label: 'Facebook', href: '#' },
  { name: 'Instagram', label: 'Instagram', href: '#' },
  { name: 'TikTok', label: 'TikTok', href: '#' },
  { name: 'YouTube', label: 'YouTube', href: '#' }
]

const footerColumns = [
  {
    title: 'Intended Parents',
    links: [
      'Find a Surrogate Parent',
      'Process for Intended Parents',
      'Surrogacy Cost',
      'Parents Application',
      'FAQ for Intended Parents'
    ]
  },
  {
    title: 'Surrogates',
    links: [
      'Surrogacy Overview',
      'Surrogate Requirements',
      'Surrogate Compensation',
      'Surrogate Application',
      'Become a Surrogate FAQ'
    ]
  },
  {
    title: 'Egg Donors',
    links: [
      'Egg Donation Overview',
      'Compensation for Egg Donors',
      'Egg Donor Process',
      'Egg Donor Application',
      'Egg Donor FAQ'
    ]
  }
]

const policies = ['Privacy', 'Cookie', 'Terms', 'Sitemap']

const currentYear = new Date().getFullYear()
</script>
