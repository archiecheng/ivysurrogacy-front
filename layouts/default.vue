<template>
  <div
    class="min-h-screen flex flex-col bg-white text-slate-800 overflow-x-hidden"
  >
    <header class="shadow-sm bg-white">
      <div class="bg-rose-400 text-white text-sm">
        <div
          class="max-w-6xl mx-auto flex flex-col gap-3 px-4 py-2 md:flex-row md:items-center md:justify-between"
        >
          <div class="flex flex-wrap items-center gap-4 text-xs sm:text-sm">
            <span class="inline-flex items-center gap-1.5">
              <Icon icon="mdi:phone" class="h-4 w-4" />
              (626)545-1617
            </span>
            <span class="inline-flex items-center gap-1.5">
              <Icon icon="mdi:email-outline" class="h-4 w-4" />
              contact@ivysurrogacy.com
            </span>
          </div>
          <div class="flex items-center gap-4 text-base">
            <a
              v-for="social in socialLinks"
              :key="social.name"
              :href="social.href"
              class="transition hover:text-white/80"
            >
              <Icon
                :icon="social.icon"
                class="h-4 w-4"
                :aria-label="social.name"
              />
            </a>
          </div>
        </div>
      </div>

      <div
        class="max-w-6xl mx-auto px-4 py-4 flex flex-col gap-4 md:flex-row md:items-center md:justify-between"
      >
        <div class="flex items-center gap-3">
          <div
            class="w-12 h-12 rounded-full bg-rose-400 text-white flex items-center justify-center font-semibold text-lg"
          >
            IVY
          </div>
          <div>
            <p class="text-lg font-semibold text-rose-500">IVY Surrogacy</p>
            <p class="text-xs text-slate-500">Building families with care</p>
          </div>
        </div>

        <nav class="flex-1 w-full">
          <ul
            class="flex flex-wrap md:flex-nowrap items-center gap-x-4 gap-y-2 text-sm font-medium text-slate-700 overflow-x-auto"
          >
            <li
              v-for="link in navLinks"
              :key="link.label"
              class="flex items-center gap-1 cursor-pointer hover:text-rose-500 transition"
            >
              <a :href="link.href">{{ link.label }}</a>
              <ChevronDownIcon
                v-if="link.dropdown"
                class="h-4 w-4 hidden sm:inline-block"
              />
            </li>
          </ul>
        </nav>

        <NuxtLink
          to="/login"
          class="hidden md:inline-flex items-center gap-2 text-sm font-semibold text-rose-500 border border-rose-200 rounded-full px-4 py-1.5 hover:bg-rose-50 transition"
        >
          Login / Signup
        </NuxtLink>
        <NuxtLink
          to="/login"
          class="md:hidden inline-flex items-center justify-center text-sm font-semibold text-rose-500 border border-rose-200 rounded-full px-4 py-1.5 hover:bg-rose-50 transition w-full"
        >
          Login / Signup
        </NuxtLink>
      </div>
    </header>

    <main class="flex-1 flex flex-col">
      <slot></slot>
    </main>

    <footer class="bg-[#d86f86] text-white mt-12">
      <div
        class="max-w-6xl mx-auto px-4 py-12 grid gap-12 lg:grid-cols-[1.4fr,repeat(3,minmax(0,1fr))] text-sm text-white/90"
      >
        <div class="space-y-4">
          <div class="flex items-center gap-4">
            <div
              class="h-16 w-16 rounded-full bg-white/30 text-white text-2xl font-semibold flex items-center justify-center"
            >
              IVY
            </div>
            <div>
              <p class="text-2xl font-semibold text-white">IVY Surrogacy</p>
              <p class="text-sm text-white/80">Growing families since 2015</p>
            </div>
          </div>
        </div>

        <div
          v-for="column in footerColumnsTop"
          :key="column.title"
          class="flex flex-col items-start space-y-2 text-left"
        >
          <p class="text-lg font-semibold text-white">{{ column.title }}</p>
          <ul class="space-y-1 list-disc pl-4 marker:text-white/70">
            <li v-for="item in column.links" :key="item">{{ item }}</li>
          </ul>
        </div>

        <div
          v-for="column in footerColumnsBottom"
          :key="column.title"
          class="flex flex-col items-start space-y-2 text-left"
        >
          <p class="text-lg font-semibold text-white">{{ column.title }}</p>
          <ul class="space-y-1 list-disc pl-4 marker:text-white/70">
            <li v-for="item in column.links" :key="item">{{ item }}</li>
          </ul>
        </div>
      </div>

      <div class="border-t border-white/20">
        <div
          class="max-w-6xl mx-auto px-4 py-4 flex flex-col gap-4 text-sm text-white/80 md:flex-row md:items-center md:justify-between"
        >
          <div class="flex gap-4 text-lg">
            <a
              v-for="social in socialLinks"
              :key="social.name"
              :href="social.href"
              class="transition hover:text-white"
            >
              <Icon :icon="social.icon" class="h-5 w-5" />
            </a>
          </div>
          <p>© {{ currentYear }} Ivy Surrogacy. All rights reserved.</p>
          <div class="flex flex-wrap items-center gap-4">
            <a
              v-for="policy in policies"
              :key="policy"
              href="#"
              class="hover:text-white transition"
            >
              {{ policy }}
            </a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ChevronDownIcon } from "@heroicons/vue/24/outline";
import { Icon } from "@iconify/vue";

const navLinks = [
  { label: "Intended Parents", href: "#", dropdown: true },
  { label: "Surrogates", href: "#", dropdown: true },
  { label: "Egg Donors", href: "#", dropdown: true },
  { label: "Resources", href: "#", dropdown: true },
  { label: "About", href: "#", dropdown: false },
  { label: "Contact", href: "#", dropdown: false },
];

const socialLinks = [
  { name: "Facebook", icon: "mdi:facebook", href: "#" },
  { name: "Instagram", icon: "mdi:instagram", href: "#" },
  { name: "TikTok", icon: "mdi:tiktok", href: "#" },
  { name: "YouTube", icon: "mdi:youtube", href: "#" },
];

const footerColumnsTop = [
  {
    title: "Contact Us",
    links: [
      "680 Brea Canyon Rd, Unit 288, Diamond Bar, CA 91789",
      "contact@ivysurrogacy.com",
      "(626)545-1617",
      "(626)270-5574",
      "9495629310",
      "9495629310",
    ],
  },
  {
    title: "Login / Sign Up",
    links: ["enchengche..", "Log Out", "About Us", "Collaborate"],
  },
  {
    title: "Resources",
    links: [
      "Blog",
      "Due Date Calculator",
      "BMI Calculator",
      "Height and Weight Converter",
      "Pregnancy Weight Gain Calculator",
      "Embryo Transfer Success Rate Calculator",
      "PGT-A euploid probability calculator",
      "Surrogacy by State",
    ],
  },
];

const footerColumnsBottom = [
  {
    title: "Intended Parents",
    links: [
      "Find a Surrogate Parents",
      "Surrogacy Parent Overview",
      "Process for Intended Parents",
      "Surrogacy Cost",
      "Parents' Application",
      "FAQ for Intended Parents",
    ],
  },
  {
    title: "Find an Egg Donor",
    links: [
      "Egg Donation Parent Overview",
      "Process for Intended Parents",
      "Egg Donation Cost",
      "Egg Donation Database",
      "Find an Egg Donor FAQ",
    ],
  },
  {
    title: "Surrogates",
    links: [
      "Surrogacy Overview",
      "Surrogate Requirements",
      "Surrogate Compensation",
      "Surrogacy Process",
      "Surrogate Application",
      "Surrogate Referral Program",
      "Become a Surrogate FAQ",
    ],
  },
  {
    title: "Egg Donors",
    links: [
      "Egg Donation Overview",
      "Compensation for Egg Donors",
      "Egg Donor Process",
      "Egg Donor Application",
      "Egg Donor FAQ",
    ],
  },
];

const policies = ["Privacy", "Cookie", "Terms", "Sitemap"];

const currentYear = new Date().getFullYear();
</script>
