<template>
  <div
    class="flex min-h-full w-full items-center justify-center bg-gradient-to-b from-[#fedce9] via-[#ffe9f3] to-[#fff6fa] px-4 py-12 sm:py-16 border-box"
  >
    <div class="w-full max-w-5xl">
      <div
        class="rounded-[36px] bg-white px-6 py-10 shadow-[0_45px_120px_rgba(236,101,150,0.18)] sm:px-8 sm:py-8 lg:px-8"
      >
        <div class="space-y-3 text-left">
          <p
            class="font-sans text-xs font-semibold tracking-[0.4em] text-[#eb7aa7]"
          >
            TELL US MORE
          </p>
          <h1
            class="font-serif text-4xl font-semibold leading-snug text-[#2b2f43] sm:text-[44px]"
          >
            Tell us more about you
          </h1>
          <p class="font-sans text-base text-[#7a768f]">
            Share a few details so we can personalize your Ivy Surrogacy
            experience.
          </p>
        </div>

        <form class="mt-6 space-y-12" @submit.prevent="handleSubmit">
          <div class="grid gap-12 md:grid-cols-2">
            <div class="space-y-8">
              <label
                class="block font-sans text-sm font-semibold text-[#6f6b88]"
              >
                <span class="mb-2 block">Name</span>
                <input
                  v-model="form.name"
                  type="text"
                  name="name"
                  placeholder="Your Name"
                  class="w-full border-b border-[#e5d8ec] bg-transparent px-0 pb-3 text-lg text-[#2b2f43] placeholder:text-[#bab3c9] transition focus:border-[#f2579d] focus:outline-none"
                />
              </label>

              <label
                class="block font-sans text-sm font-semibold text-[#6f6b88]"
              >
                <span class="mb-2 block">Mobile phone</span>
                <input
                  v-model="form.mobile"
                  type="tel"
                  name="mobile"
                  placeholder="Your Phone Number"
                  class="w-full border-b border-[#e5d8ec] bg-transparent px-0 pb-3 text-lg text-[#2b2f43] placeholder:text-[#bab3c9] transition focus:border-[#f2579d] focus:outline-none"
                />
              </label>
            </div>

            <div class="space-y-8">
              <div class="space-y-4">
                <p class="font-sans text-sm font-semibold text-[#6f6b88]">
                  Looking for
                </p>
                <div class="flex flex-wrap gap-3">
                  <button
                    v-for="option in lookingOptions"
                    :key="option.value"
                    type="button"
                    class="rounded-full border px-5 py-2.5 font-sans text-sm font-semibold transition-all duration-200"
                    :class="[
                      option.value === form.lookingFor
                        ? 'border-[#f2579d] bg-[#f2579d] text-white shadow-[0_12px_25px_rgba(242,87,157,0.25)]'
                        : 'border-[#e5d8ec] bg-white text-[#6c6786] hover:border-[#f2579d] hover:text-[#f2579d] hover:shadow-[0_8px_18px_rgba(242,87,157,0.15)]',
                    ]"
                    @click="selectLookingFor(option.value)"
                  >
                    {{ option.label }}
                  </button>
                </div>
              </div>

              <div class="space-y-4">
                <p class="font-sans text-sm font-semibold text-[#6f6b88]">
                  Preferred language
                </p>
                <div class="flex flex-wrap gap-3">
                  <button
                    v-for="option in languageOptions"
                    :key="option.value"
                    type="button"
                    class="rounded-full border px-5 py-2.5 font-sans text-sm font-semibold transition-all duration-200"
                    :class="[
                      option.value === form.language
                        ? 'border-[#f2579d] bg-[#f2579d] text-white shadow-[0_12px_25px_rgba(242,87,157,0.25)]'
                        : 'border-[#e5d8ec] bg-white text-[#6c6786] hover:border-[#f2579d] hover:text-[#f2579d] hover:shadow-[0_8px_18px_rgba(242,87,157,0.15)]',
                    ]"
                    @click="selectLanguage(option.value)"
                  >
                    {{ option.label }}
                  </button>
                </div>
              </div>
            </div>
          </div>

          <div class="flex flex-col gap-4 md:flex-row md:items-center md:gap-8">
            <button
              type="submit"
              class="w-full rounded-full bg-[#f2579d] px-8 py-4 text-center font-sans text-lg font-semibold text-white shadow-[0_20px_45px_rgba(242,87,157,0.3)] transition hover:bg-[#e0468d]"
            >
              Submit
            </button>
            <button
              type="button"
              class="font-sans text-sm font-semibold text-[#8d88a4] transition hover:text-[#f2579d]"
              @click="resetForm"
            >
              Cancel
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
useHead({
  bodyAttrs: {
    class: ["futureparents-hide-scrollbar"],
  },
});

type Option = {
  label: string;
  value: string;
};

type FormState = {
  name: string;
  mobile: string;
  lookingFor: string;
  language: string;
};

const lookingOptions: Option[] = [
  { label: "Surrogate", value: "surrogate" },
  { label: "Egg Donor", value: "egg-donor" },
  { label: "Both", value: "both" },
];

const languageOptions: Option[] = [
  { label: "English", value: "english" },
  { label: "Chinese", value: "chinese" },
  { label: "Spanish", value: "spanish" },
];

const form = reactive<FormState>({
  name: "",
  mobile: "",
  lookingFor: lookingOptions[0].value,
  language: languageOptions[0].value,
});

const selectLookingFor = (value: string) => {
  form.lookingFor = value;
};

const selectLanguage = (value: string) => {
  form.language = value;
};

const resetForm = () => {
  form.name = "";
  form.mobile = "";
  form.lookingFor = lookingOptions[0].value;
  form.language = languageOptions[0].value;
};

const handleSubmit = () => {
  console.log("Future parents form submitted", { ...form });
};
</script>

<style scoped>
:global(body.futureparents-hide-scrollbar) {
  overflow-y: auto;
  scrollbar-width: none;
}

:global(body.futureparents-hide-scrollbar::-webkit-scrollbar) {
  display: none;
}
</style>
