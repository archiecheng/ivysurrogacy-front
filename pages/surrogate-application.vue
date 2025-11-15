<template>
  <section class="mx-auto max-w-[1100px] px-4 py-12 lg:py-16">
    <header class="flex flex-col gap-4 border-b border-gray-100 pb-6 lg:flex-row lg:items-end lg:justify-between">
      <div>
        <p class="text-xs font-semibold uppercase tracking-[0.35em] text-rose-400">Application</p>
        <h1 class="text-3xl font-bold text-slate-900">Surrogate Full Application</h1>
        <p class="text-sm text-slate-500">Complete each step to submit your full surrogate application.</p>
      </div>
      <a href="#" class="text-sm font-semibold text-rose-500 hover:text-rose-600">Change your password</a>
    </header>

    <div class="mt-8 space-y-4 lg:hidden">
      <div class="rounded-xl border border-rose-100 bg-rose-50 px-4 py-3 text-sm font-semibold text-rose-600">
        Step {{ currentStepIndex + 1 }} of {{ steps.length }} — {{ currentStep.label }}
      </div>
      <select
        v-model="currentStepId"
        class="w-full rounded-lg border border-rose-100 bg-white px-4 py-3 text-sm text-slate-700 focus:border-rose-200 focus:ring-2 focus:ring-rose-100"
      >
        <option v-for="step in steps" :key="step.id" :value="step.id">
          {{ step.label }}
        </option>
      </select>
    </div>

    <div class="mt-8 lg:flex lg:items-start lg:gap-6">
      <aside class="hidden w-64 shrink-0 lg:block lg:sticky lg:top-24">
        <nav class="flex flex-col gap-2">
          <button
            v-for="(step, index) in steps"
            :key="step.id"
            type="button"
            class="flex items-center justify-between rounded-2xl px-4 py-3 text-left text-sm font-semibold transition"
            :class="[
              currentStepId === step.id
                ? 'bg-[#fb7185] text-white shadow-lg shadow-[#fb7185]/40'
                : 'bg-slate-50 text-slate-600 hover:bg-slate-100',
            ]"
            @click="selectStep(step.id)"
          >
            <span>{{ index + 1 }}. {{ step.label }}</span>
            <CheckIcon
              v-if="completedSteps.includes(step.id)"
              class="h-5 w-5"
              :class="currentStepId === step.id ? 'text-white' : 'text-rose-500'"
            />
          </button>
        </nav>
      </aside>

      <div class="flex-1 lg:ml-6 w-[40rem]">
        <form class="form-card space-y-10" @submit.prevent="handlePrimary">
          <div>
            <p class="text-xs font-semibold uppercase tracking-[0.35em] text-rose-400">
              Step {{ currentStepIndex + 1 }} of {{ steps.length }}
            </p>
            <h2 class="mt-3 text-2xl font-semibold text-slate-900">{{ currentStep.title }}</h2>
            <p class="text-sm text-slate-500">{{ currentStep.description }}</p>
          </div>

          <div v-if="currentStepId === 'contact'" class="space-y-10">
            <div class="grid gap-6 md:grid-cols-2">
              <div class="field">
                <label>First name <span class="required">*</span></label>
                <input type="text" class="input" placeholder="Jane" />
              </div>
              <div class="field">
                <label>Last name <span class="required">*</span></label>
                <input type="text" class="input" placeholder="Futureparent" />
              </div>
              <div class="field md:col-span-2">
                <label>Middle name</label>
                <input type="text" class="input" placeholder="Optional" />
              </div>
              <div class="field md:col-span-2">
                <label>Date of birth <span class="required">*</span></label>
                <input type="date" class="input" />
              </div>
            </div>

            <div class="space-y-4">
              <h3 class="section-heading">Address</h3>
              <div class="grid gap-6 md:grid-cols-2">
                <div class="field md:col-span-2">
                  <label>Street address <span class="required">*</span></label>
                  <input type="text" class="input" placeholder="Street and number" />
                </div>
                <div class="field md:col-span-2">
                  <label>Address line 2</label>
                  <input type="text" class="input" placeholder="Apartment, suite, etc." />
                </div>
                <div class="field">
                  <label>City <span class="required">*</span></label>
                  <input type="text" class="input" />
                </div>
                <div class="field">
                  <label>State <span class="required">*</span></label>
                  <select class="input">
                    <option value="">Select</option>
                    <option v-for="state in usaStates" :key="state" :value="state">
                      {{ state }}
                    </option>
                  </select>
                </div>
                <div class="field">
                  <label>Zip code <span class="required">*</span></label>
                  <input type="text" class="input" />
                </div>
              </div>
            </div>

            <div class="grid gap-6 md:grid-cols-2">
              <div class="field">
                <label>Cell phone number <span class="required">*</span></label>
                <input type="text" class="input" placeholder="+1 (555) 555-5555" />
                <p class="helper">We’ll use this if we need to reach you quickly.</p>
              </div>
              <div class="field">
                <label>Email address <span class="required">*</span></label>
                <input type="email" class="input" placeholder="you@example.com" />
              </div>
            </div>

            <div class="space-y-4">
              <h3 class="section-heading">Emergency contact</h3>
              <div class="grid gap-6 md:grid-cols-2">
                <div class="field">
                  <label>Name</label>
                  <input type="text" class="input" placeholder="Full name" />
                </div>
                <div class="field">
                  <label>Relationship</label>
                  <input type="text" class="input" placeholder="Sister, partner..." />
                </div>
                <div class="field">
                  <label>Phone number</label>
                  <input type="text" class="input" placeholder="+1 (555) 000-1234" />
                </div>
                <div class="field">
                  <label>Address</label>
                  <input type="text" class="input" placeholder="City, State" />
                </div>
              </div>
            </div>

            <div class="field max-w-sm">
              <label>How did you hear about us?</label>
              <select class="input">
                <option value="">Select an option</option>
                <option v-for="option in hearOptions" :key="option" :value="option">
                  {{ option }}
                </option>
              </select>
            </div>

            <RadioGroup label="Do you allow approved photos to be used online?" :options="yesNoOptions" />
            <RadioGroup label="Are you currently listed with any other agencies?" :options="yesNoOptions" />
          </div>

          <div v-else-if="currentStepId === 'personal'" class="space-y-8">
            <p class="text-sm text-slate-500">
              Tell us more about yourself and your background so we can match you with the right intended parents.
            </p>
            <div class="grid gap-6 md:grid-cols-2">
              <div class="field">
                <label>Are you a U.S. citizen?</label>
                <RadioInline :options="yesNoOptions" />
              </div>
              <div class="field">
                <label>What is your race?</label>
                <select class="input">
                  <option value="">Select</option>
                  <option v-for="option in ethnicityOptions" :key="option" :value="option">
                    {{ option }}
                  </option>
                </select>
              </div>
              <div class="field">
                <label>Marital status</label>
                <select class="input">
                  <option value="">Select</option>
                  <option v-for="option in maritalOptions" :key="option" :value="option">
                    {{ option }}
                  </option>
                </select>
              </div>
              <div class="field">
                <label>How many biological children do you have?</label>
                <input type="number" class="input" min="0" />
              </div>
              <div class="field md:col-span-2">
                <label>What languages do you speak fluently?</label>
                <input type="text" class="input" placeholder="English, Spanish..." />
              </div>
            </div>

            <div class="grid gap-6 md:grid-cols-2">
              <div class="field md:col-span-2">
                <label>
                  Is your schedule flexible? (You might be required to attend multiple medical appointments.)
                </label>
                <RadioInline :options="yesNoOptions" />
              </div>
              <div class="field">
                <label>Do you have a valid driver’s license?</label>
                <RadioInline :options="yesNoOptions" />
              </div>
              <div class="field">
                <label>Do you have reliable transportation?</label>
                <RadioInline :options="yesNoOptions" />
              </div>
              <div class="field">
                <label>Do you have car insurance?</label>
                <RadioInline :options="yesNoOptions" />
              </div>
              <div class="field">
                <label>What is the name of your health insurance carrier?</label>
                <input type="text" class="input" placeholder="Insurance provider name" />
              </div>
            </div>
          </div>

          <div v-else-if="currentStepId === 'education'" class="space-y-10">
            <div class="space-y-4">
              <h3 class="section-heading">Education</h3>
              <div class="grid gap-6 md:grid-cols-2">
                <div class="field md:col-span-2">
                  <label>What is the highest-level education you have completed?</label>
                  <select class="input">
                    <option value="">Select</option>
                    <option v-for="option in educationOptions" :key="option" :value="option">
                      {{ option }}
                    </option>
                  </select>
                </div>
                <div class="field md:col-span-2">
                  <label>Do you have plans on furthering your education?</label>
                  <RadioInline :options="yesNoOptions" />
                </div>
              </div>
            </div>

            <div class="space-y-4">
              <h3 class="section-heading">Employment</h3>
              <div class="grid gap-6 md:grid-cols-2">
                <div class="field md:col-span-2">
                  <label>Are you currently employed?</label>
                  <RadioInline :options="yesNoOptions" />
                </div>
                <div class="field">
                  <label>Occupation</label>
                  <input type="text" class="input" placeholder="Job title (if applicable)" />
                </div>
                <div class="field">
                  <label>Employer</label>
                  <input type="text" class="input" placeholder="Company name" />
                </div>
                <div class="field">
                  <label>Employment status</label>
                  <select class="input">
                    <option value="">Select</option>
                    <option v-for="option in employmentOptions" :key="option" :value="option">
                      {{ option }}
                    </option>
                  </select>
                </div>
                <div class="field">
                  <label>Typical work schedule</label>
                  <input type="text" class="input" placeholder="Mon–Fri 9am-5pm" />
                </div>
              </div>
            </div>
          </div>

          <div v-else-if="currentStepId === 'family'" class="space-y-8">
            <p class="text-sm text-slate-500">A supportive home life makes a huge difference in the surrogacy journey.</p>
            <RadioGroup label="Do you have a spouse or significant other?" :options="yesNoOptions" />
            <RadioGroup label="Does your family support your decision to become a gestational carrier?" :options="yesNoOptions" />
            <div class="field">
              <label>Who would help if you were ordered to be on bed rest for a period of time?</label>
              <textarea rows="4" class="input" placeholder="List names, relationships, and how they would support you."></textarea>
            </div>
            <div class="field">
              <label>Do you anticipate any difficulties in becoming a surrogate?</label>
              <textarea rows="4" class="input" placeholder="Share any concerns, scheduling challenges, or questions."></textarea>
            </div>
            <div class="field">
              <label>Describe your current living conditions.</label>
              <textarea rows="4" class="input" placeholder="Home type, neighborhood, number of rooms, etc."></textarea>
            </div>
            <div class="field">
              <label>Please list everyone living in your household including ages and relationship.</label>
              <textarea rows="4" class="input" placeholder="Example: Alex (Partner, 35), Mia (Daughter, 6)..."></textarea>
            </div>
            <RadioGroup label="Do you have any pets at home?" :options="yesNoOptions" />
          </div>

          <div v-else-if="currentStepId === 'medical'" class="space-y-8">
            <div class="grid gap-6 md:grid-cols-2">
              <div class="field">
                <label>Your blood type</label>
                <select class="input">
                  <option value="">Select</option>
                  <option v-for="type in bloodTypes" :key="type" :value="type">
                    {{ type }}
                  </option>
                </select>
              </div>
              <div class="field">
                <label>Your Rh factor</label>
                <select class="input">
                  <option value="">Select</option>
                  <option value="Positive">Positive</option>
                  <option value="Negative">Negative</option>
                  <option value="Unknown">Unknown</option>
                </select>
              </div>
              <div class="field">
                <label>Your height</label>
                <div class="flex gap-3">
                  <select class="input">
                    <option value="">ft</option>
                    <option v-for="ft in heightFeet" :key="ft" :value="ft">
                      {{ ft }} ft
                    </option>
                  </select>
                  <select class="input">
                    <option value="">in</option>
                    <option v-for="inch in heightInches" :key="inch" :value="inch">
                      {{ inch }} in
                    </option>
                  </select>
                </div>
              </div>
              <div class="field">
                <label>Your weight</label>
                <input type="number" class="input" placeholder="lbs" />
              </div>
            </div>

            <RadioGroup label="Do you drink alcoholic beverages?" :options="yesNoOptions" />
            <RadioGroup label="Do you or anyone in your household smoke?" :options="yesNoOptions" />
            <RadioGroup label="Have you or anyone in your household used illicit drugs?" :options="yesNoOptions" />
            <div class="field">
              <label>When was the last time you had any form of tobacco, marijuana, or any illicit drugs?</label>
              <textarea rows="3" class="input" placeholder="Share dates or details if applicable."></textarea>
            </div>

            <RadioGroup label="Are you taking any medication?" :options="yesNoOptions" />
            <RadioGroup label="Are you currently being treated for any medical conditions?" :options="yesNoOptions" />
            <div class="field">
              <label>Please list any significant illnesses you have had.</label>
              <textarea rows="3" class="input"></textarea>
            </div>
            <div class="field">
              <label>Please list any hospitalization or operations you have had. (Excluding childbirth.)</label>
              <textarea rows="3" class="input"></textarea>
            </div>
            <div class="field">
              <label>How close are you to the nearest hospital? What is the name/city?</label>
              <textarea rows="3" class="input"></textarea>
            </div>

            <RadioGroup label="Have you ever taken medications for depression or anxiety?" :options="yesNoOptions" />
            <RadioGroup label="Have you or any partners been hospitalized for psychiatric illness?" :options="yesNoOptions" />
            <RadioGroup label="Have you been immunized for Hepatitis B?" :options="['Yes', 'No', 'Unsure']" />

            <div class="field">
              <label>Have you ever been diagnosed with the following diseases?</label>
              <div class="grid gap-4 md:grid-cols-2">
                <RadioGroup v-for="disease in diseaseList" :key="disease" :label="disease" :options="yesNoOptions" />
              </div>
            </div>

            <RadioGroup label="Has your partner/spouse been diagnosed with herpes, gonorrhea, chlamydia, syphilis, HPV, or genital warts?" :options="['Yes', 'No', 'Unsure']" />
          </div>

          <div v-else-if="currentStepId === 'pregnancy'" class="space-y-8">
            <div class="space-y-4">
              <h3 class="section-heading">Pregnancy history</h3>
              <p class="text-sm text-slate-500">Please list your pregnancies from earliest to most recent.</p>
              <div class="grid gap-6 md:grid-cols-2">
                <div class="field">
                  <label>Pregnancy number</label>
                  <input type="number" class="input" min="1" />
                </div>
                <div class="field">
                  <label>Own or surrogacy</label>
                  <select class="input">
                    <option value="">Select</option>
                    <option value="own">Own pregnancy</option>
                    <option value="surrogacy">Surrogacy</option>
                  </select>
                </div>
                <div class="field">
                  <label>Vaginal or C-section</label>
                  <select class="input">
                    <option value="">Select</option>
                    <option value="vaginal">Vaginal</option>
                    <option value="c-section">C-section</option>
                  </select>
                </div>
                <div class="field">
                  <label>Number of babies</label>
                  <input type="number" class="input" min="1" />
                </div>
                <div class="field">
                  <label>Delivery date</label>
                  <input type="date" class="input" />
                </div>
                <div class="field">
                  <label>Gender</label>
                  <input type="text" class="input" placeholder="Boy, Girl, Twins..." />
                </div>
                <div class="field">
                  <label>Birth weight</label>
                  <input type="text" class="input" placeholder="lbs/oz" />
                </div>
                <div class="field">
                  <label>Weeks at birth</label>
                  <input type="number" class="input" />
                </div>
                <div class="field md:col-span-2">
                  <label>Complications</label>
                  <textarea rows="3" class="input" placeholder="Include any NICU time or special notes."></textarea>
                </div>
              </div>
            </div>

            <RadioGroup label="Have you ever had an abortion?" :options="yesNoOptions" />
            <RadioGroup label="Have you ever had a miscarriage?" :options="yesNoOptions" />

            <div class="space-y-4">
              <h3 class="section-heading">Have you ever experienced the following conditions?</h3>
              <div class="grid gap-4 md:grid-cols-2">
                <RadioGroup v-for="condition in pregnancyConditions" :key="condition" :label="condition" :options="yesNoOptions" />
              </div>
            </div>

            <RadioGroup label="Are you currently breastfeeding?" :options="yesNoOptions" />
            <RadioGroup label="Are you sexually active?" :options="yesNoOptions" />
            <RadioGroup label="Are you using birth control?" :options="yesNoOptions" />
            <RadioGroup label="Do you have regular monthly menstrual cycles?" :options="yesNoOptions" />

            <div class="grid gap-6 md:grid-cols-2">
              <div class="field">
                <label>Date of your last menstrual cycle</label>
                <input type="date" class="input" />
              </div>
              <div class="field">
                <label>When did you last see your OB/GYN?</label>
                <input type="date" class="input" />
              </div>
            </div>

            <div class="field">
              <label>What is the date of your last Pap smear? What was the result?</label>
              <textarea rows="3" class="input" placeholder="Include any follow-up needed."></textarea>
            </div>
            <div class="field">
              <label>Please list any reproductive illness you have ever experienced.</label>
              <textarea rows="3" class="input"></textarea>
            </div>
          </div>

          <div v-else-if="currentStepId === 'characteristics'" class="space-y-8">
            <div class="field">
              <label>Why do you want to be a surrogate? What message would you like to give to your intended parents?</label>
              <textarea rows="4" class="input"></textarea>
            </div>
            <div class="field">
              <label>Please describe your personality and character.</label>
              <textarea rows="4" class="input"></textarea>
            </div>
            <div class="field">
              <label>What are your hobbies, interests, and talents?</label>
              <textarea rows="4" class="input"></textarea>
            </div>
            <div class="field">
              <label>What does your daily diet consist of?</label>
              <textarea rows="4" class="input"></textarea>
            </div>
          </div>

          <div v-else-if="currentStepId === 'decisions'" class="space-y-8">
            <div class="field space-y-4">
              <label>Are you willing to work with intended parents?</label>
              <div class="grid gap-4 md:grid-cols-2">
                <RadioGroup label="Heterosexual couples (male/female)" :options="yesNoOptions" />
                <RadioGroup label="Heterosexual individuals" :options="yesNoOptions" />
                <RadioGroup label="Same-sex couples" :options="yesNoOptions" />
                <RadioGroup label="Same-sex individuals" :options="yesNoOptions" />
              </div>
            </div>

            <RadioGroup label="Are you willing to work with international intended parents? (No travel required.)" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to carry for intended parents who carry Hep B virus?" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to carry for intended parents who recovered from Hep B but no longer carry it?" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to carry for intended parents who have HIV?" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to carry for recipients who used donor eggs or donor sperm?" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to carry for recipients with a different religion than your own?" :options="yesNoOptions" />
            <div class="field">
              <label>What kind of relationship do you want with intended parents during conception and pregnancy?</label>
              <textarea rows="3" class="input"></textarea>
            </div>

            <div class="field">
              <label>How many maximum number of babies are you willing to carry?</label>
              <select class="input">
                <option value="">Select</option>
                <option value="1">1</option>
                <option value="2">2</option>
              </select>
            </div>

            <RadioGroup label="Based on a physician’s recommendation in the best interest of the child, are you willing to terminate the pregnancy?" :options="yesNoOptions" />
            <RadioGroup label="Based on the recipients’ preference in the best interest of the child, are you willing to terminate the pregnancy?" :options="yesNoOptions" />
            <RadioGroup label="Are you okay with the reduction of multiples if medically necessary?" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to allow fetal surgery if a doctor recommends it?" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to undergo invasive procedures if medically necessary (D&C, amniocentesis, CVS)?" :options="yesNoOptions" />
            <RadioGroup label="Are you willing to pump breast milk after birth?" :options="yesNoOptions" />
            <RadioGroup label="Would you be comfortable if intended parents attended OB appointments with you?" :options="yesNoOptions" />
            <div class="field">
              <label>Who are you willing to have in the delivery room?</label>
              <textarea rows="3" class="input"></textarea>
            </div>
            <RadioGroup label="IVF medications may involve injectables. Do you agree to take all medications required?" :options="yesNoOptions" />
          </div>

          <div v-else-if="currentStepId === 'legal'" class="space-y-8">
            <RadioGroup label="Have you or anyone in your household been accused, convicted, or cleared of a felony?" :options="yesNoOptions" />
            <div class="field">
              <label>Have you or anyone in your household been accused, convicted, or cleared of the following crimes (rape, sexual assault, lewd act with a minor, sexual abuse, child molestation, domestic violence)? Please explain who, the type of crime, date, and outcome.</label>
              <textarea rows="4" class="input"></textarea>
            </div>
            <RadioGroup label="Have you or anyone in your household had a temporary or permanent restraining order or order of protection against anyone in the past 5 years?" :options="yesNoOptions" />
            <RadioGroup label="Has anyone issued a temporary or permanent restraining order or order of protection against you or anyone in your household in the past 5 years?" :options="yesNoOptions" />
            <RadioGroup label="Are you or anyone in your household involved in any legal proceedings?" :options="yesNoOptions" />
          </div>

          <div v-else-if="currentStepId === 'photos'" class="space-y-6">
            <p class="text-sm text-slate-500">Upload clear, recent photos. These help intended parents connect with you.</p>
            <div class="grid gap-6 md:grid-cols-2">
              <UploadTile label="Featured image" helper="Headshot or clear portrait." />
              <UploadTile label="Gallery upload" helper="Add multiple photos (drag & drop or browse)." />
            </div>
          </div>

          <div v-else-if="currentStepId === 'authorization'" class="space-y-8">
            <p class="text-sm text-slate-500">By clicking Submit, I hereby:</p>
            <ul class="list-disc space-y-2 pl-5 text-sm text-slate-600">
              <li>
                Acknowledge that all information submitted is true and correct to the best of my knowledge and belief, and that no material information has been knowingly omitted. I understand that inaccurate information may make me ineligible to become a surrogate with Ivy Surrogacy.
              </li>
              <li>
                Authorize Ivy Surrogacy, its agents, and representatives to show my profile and photographs to potential parents for the purpose of promoting me as a surrogate.
              </li>
              <li>
                Represent that I have read this information in its entirety, have been advised to discuss it with an attorney if needed, and that all questions have been answered satisfactorily.
              </li>
            </ul>
            <CheckboxGroup label="Final authorization" :options="authorizationOptions" />
          </div>

          <div class="flex flex-col gap-3 border-t border-gray-100 pt-6 sm:flex-row sm:items-center sm:justify-between">
            <button type="button" class="btn-secondary sm:w-auto" :disabled="currentStepIndex === 0" @click="goBack">
              Back
            </button>
            <div class="flex flex-col gap-3 sm:flex-row sm:gap-4">
              <button type="button" class="btn-secondary" @click="handleSaveDraft">Save draft</button>
              <button type="submit" class="btn-primary">
                {{ isLastStep ? 'Submit application' : 'Save and next step' }}
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { CheckIcon } from '@heroicons/vue/24/outline'
import { computed, defineComponent, h, ref } from 'vue'

const steps = [
  { id: 'contact', label: 'Contact Info', title: 'Contact Information', description: 'We’ll reach out to you using this information.' },
  { id: 'personal', label: 'Personal Info', title: 'Personal Information', description: 'Tell us more about yourself and your background.' },
  { id: 'education', label: 'Education/Employment', title: 'Education & Employment', description: 'A quick snapshot of your education and work.' },
  { id: 'family', label: 'Family Support', title: 'Family Support', description: 'We want to make sure your inner circle is on board.' },
  { id: 'medical', label: 'Medical Info', title: 'Medical Information', description: 'Share important health details for our review.' },
  { id: 'pregnancy', label: 'Pregnancy Info', title: 'Pregnancy History', description: 'Detail your pregnancies and delivery experience.' },
  { id: 'characteristics', label: 'Characteristics', title: 'Characteristics', description: 'Let intended parents get a feel for who you are.' },
  { id: 'decisions', label: 'Decisions', title: 'Preferences & Decisions', description: 'Share your preferences so we can match you better.' },
  { id: 'legal', label: 'Legal Information', title: 'Legal Information', description: 'Review the legal expectations of surrogacy.' },
  { id: 'photos', label: 'Photos', title: 'Photos & Uploads', description: 'Upload recent photos to complete your profile.' },
  { id: 'authorization', label: 'Authorization', title: 'Authorization & Submission', description: 'Confirm everything looks right and submit.' },
] as const

type StepId = (typeof steps)[number]['id']

const currentStepId = ref<StepId>('contact')
const completedSteps = ref<StepId[]>([])

const currentStepIndex = computed(() => steps.findIndex((step) => step.id === currentStepId.value))
const currentStep = computed(() => steps[currentStepIndex.value])
const isLastStep = computed(() => currentStepIndex.value === steps.length - 1)

const selectStep = (id: StepId) => {
  currentStepId.value = id
}

const markStepComplete = (id: StepId) => {
  if (!completedSteps.value.includes(id)) {
    completedSteps.value.push(id)
  }
}

const goBack = () => {
  if (currentStepIndex.value === 0) return
  currentStepId.value = steps[currentStepIndex.value - 1].id
}

const handlePrimary = () => {
  markStepComplete(currentStepId.value)
  if (isLastStep.value) {
    window.alert('Application submitted! 📮')
    return
  }
  currentStepId.value = steps[currentStepIndex.value + 1].id
}

const handleSaveDraft = () => {
  window.alert('Your progress has been saved as a draft.')
}

const yesNoOptions = ['Yes', 'No']
const supportOptions = ['Yes, fully supportive', 'Yes, but unsure', 'No']
const maritalOptions = ['Single', 'Married', 'Domestic partnership', 'Divorced', 'Widowed']
const languageOptions = ['English', 'Spanish', 'Chinese', 'Portuguese', 'Other']
const ethnicityOptions = ['Asian', 'Black/African descent', 'Latina/Hispanic', 'White/Caucasian', 'Mixed']
const educationOptions = ['High school', "Associate's", "Bachelor's", "Master's", 'Doctorate', 'Other']
const employmentOptions = ['Full-time', 'Part-time', 'Self-employed', 'Stay-at-home', 'Student', 'Not employed']
const personalityOptions = ['Compassionate', 'Detail-oriented', 'Optimistic', 'Creative', 'Calm under pressure', 'Adventurous']
const lifestyleOptions = ['Active lifestyle', 'Mindfulness/meditation', 'Volunteering', 'Outdoor lover', 'Foodie', 'Homebody']
const preferenceOptions = ['Married couples', 'Single intended parents', 'LGBTQ+ parents', 'International parents', 'Open to all']
const pregnancyConditions = [
  'Gestational diabetes',
  'Hypertension',
  'Toxemia',
  'Placenta previa',
  'Pre-eclampsia',
  'Placenta abruption',
  'Post-partum depression',
  'Pre-term labor',
  'Short cervix',
  'Cholestasis',
  'Bedrest',
]
const medicalDecisionOptions = [
  'Selective reduction if requested',
  'C-section if medically necessary',
  'Carrying multiples',
  'Blood transfusions',
  'Receiving physician-recommended vaccines',
]
const legalOptions = [
  'I have read the Ivy Surrogacy program overview.',
  'I understand legal counsel may be required.',
  'I agree to communicate openly and honestly.',
  'I agree to medical appointments and instructions.',
]
const authorizationOptions = [
  'I confirm the information provided is accurate.',
  'I understand this submission is not a legal contract.',
  'I consent to Ivy Surrogacy reviewing my information.',
]
const hearOptions = ['Referral', 'Social media', 'Search engine', 'Event', 'Other']
const usaStates = ['Arizona', 'California', 'Colorado', 'Nevada', 'New Mexico', 'Oregon', 'Texas', 'Washington']
const bloodTypes = ['A', 'B', 'AB', 'O']
const heightFeet = [4, 5, 6]
const heightInches = Array.from({ length: 12 }, (_, i) => i)
const diseaseList = ['Herpes', 'Gonorrhea', 'Chlamydia', 'Syphilis', 'HPV', 'Genital warts']

const RadioGroup = defineComponent({
  name: 'RadioGroup',
  props: {
    label: { type: String, required: true },
    options: { type: Array as () => string[], required: true },
  },
  setup(props) {
    const model = ref('')
    const handleChange = (event: Event) => {
      const target = event.target as HTMLInputElement
      model.value = target.value
    }
    return () =>
      h('fieldset', { class: 'flex flex-col gap-3 text-sm' }, [
        h('legend', { class: 'font-semibold text-slate-700' }, props.label),
        h(
          'div',
          { class: 'flex flex-wrap gap-3' },
          props.options.map((option) =>
            h(
              'label',
              {
                class: [
                  'inline-flex items-center gap-2 rounded-full border px-4 py-2 text-sm font-medium transition',
                  model.value === option
                    ? 'border-rose-300 bg-rose-50 text-rose-600'
                    : 'border-slate-200 bg-white text-slate-600 hover:border-rose-200',
                ],
              },
              [
                h('input', {
                  type: 'radio',
                  class: 'text-rose-500 focus:ring-rose-200',
                  value: option,
                  checked: model.value === option,
                  onChange: handleChange,
                }),
                h('span', option),
              ]
            )
          )
        ),
      ])
  },
})

const CheckboxGroup = defineComponent({
  name: 'CheckboxGroup',
  props: {
    label: { type: String, required: true },
    options: { type: Array as () => string[], required: true },
  },
  setup(props) {
    const selected = ref<string[]>([])
    const toggle = (option: string) => {
      if (selected.value.includes(option)) {
        selected.value = selected.value.filter((item) => item !== option)
      } else {
        selected.value = [...selected.value, option]
      }
    }
    return () =>
      h('fieldset', { class: 'flex flex-col gap-3 text-sm' }, [
        h('legend', { class: 'font-semibold text-slate-700' }, props.label),
        h(
          'div',
          { class: 'flex flex-wrap gap-3' },
          props.options.map((option) =>
            h(
              'button',
              {
                type: 'button',
                class: [
                  'rounded-full border px-4 py-2 text-sm font-medium transition',
                  selected.value.includes(option)
                    ? 'border-rose-300 bg-rose-50 text-rose-600 shadow-sm'
                    : 'border-slate-200 bg-white text-slate-600 hover:border-rose-200',
                ],
                onClick: () => toggle(option),
              },
              option
            )
          )
        ),
      ])
  },
})

const RadioInline = defineComponent({
  name: 'RadioInline',
  props: {
    options: { type: Array as () => string[], required: true },
  },
  setup(props) {
    const model = ref('')
    const handleChange = (event: Event) => {
      const input = event.target as HTMLInputElement
      model.value = input.value
    }
    return () =>
      h(
        'div',
        { class: 'flex items-center gap-4 text-sm text-slate-600' },
        props.options.map((option) =>
          h(
            'label',
            { class: 'inline-flex items-center gap-2' },
            [
              h('input', {
                type: 'radio',
                class: 'text-rose-500 focus:ring-rose-200',
                value: option,
                checked: model.value === option,
                onChange: handleChange,
              }),
              h('span', option),
            ]
          )
        )
      )
  },
})

const UploadTile = defineComponent({
  name: 'UploadTile',
  props: {
    label: { type: String, required: true },
    helper: { type: String, default: '' },
  },
  setup(props) {
    const fileName = ref('')
    const handleUpload = (event: Event) => {
      const input = event.target as HTMLInputElement
      if (input?.files?.length) {
        fileName.value = input.files[0].name
      }
    }
    return () =>
      h('div', { class: 'flex flex-col gap-2 rounded-2xl border border-dashed border-rose-200 bg-rose-50/60 p-4 text-sm text-slate-600' }, [
        h('p', { class: 'font-semibold text-slate-800' }, props.label),
        props.helper ? h('p', { class: 'text-xs text-slate-500' }, props.helper) : null,
        h(
          'label',
          {
            class:
              'mt-2 inline-flex items-center justify-center gap-2 rounded-full bg-white px-4 py-2 text-sm font-semibold text-rose-500 shadow-sm ring-1 ring-rose-100 hover:bg-rose-50',
          },
          [
            h('input', { type: 'file', class: 'hidden', onChange: handleUpload }),
            'Upload / Replace',
          ]
        ),
        fileName.value ? h('p', { class: 'text-xs text-rose-500' }, `Uploaded: ${fileName.value}`) : null,
      ])
  },
})
</script>

<style scoped>
.form-card {
  @apply w-full max-w-[720px] rounded-[32px] border border-rose-50 bg-white p-6 shadow-[0_35px_70px_rgba(201,58,103,0.08)] md:p-10;
}

.input {
  @apply w-full rounded-xl border border-slate-200 bg-white px-4 py-2.5 text-sm text-slate-700 placeholder:text-slate-400 focus:border-rose-200 focus:outline-none focus:ring-2 focus:ring-rose-100 transition;
}

.section-heading {
  @apply text-xs font-semibold uppercase tracking-[0.35em] text-rose-400;
}

.btn-primary {
  @apply inline-flex items-center justify-center rounded-full bg-rose-500 px-6 py-3 text-sm font-semibold text-white shadow-lg shadow-rose-200 transition hover:bg-rose-600 focus:outline-none focus:ring-2 focus:ring-rose-200;
}

.btn-secondary {
  @apply inline-flex items-center justify-center rounded-full border border-slate-200 px-6 py-3 text-sm font-semibold text-slate-600 transition hover:border-rose-200 hover:text-rose-500 disabled:cursor-not-allowed disabled:border-slate-100 disabled:text-slate-300;
}

.field {
  @apply flex flex-col gap-2 text-sm;
}

.field label {
  @apply font-semibold text-slate-700;
}

.helper {
  @apply text-xs text-slate-400;
}

.required {
  @apply text-rose-500;
}
</style>
