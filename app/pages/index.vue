<script setup lang="ts">
const { data: page } = await useAsyncData('index', () => queryCollection('index').first())

const title = page.value?.seo?.title || page.value?.title
const description = page.value?.seo?.description || page.value?.description

useSeoMeta({
  titleTemplate: '',
  title,
  ogTitle: title,
  description,
  ogDescription: description
})

const toast = useToast()
const email = ref('')
const loading = ref(false)

function onSubmit() {
  loading.value = true
  setTimeout(() => {
    loading.value = false
    toast.add({
      title: 'Subscribed!',
      description: 'You\'ve been added to the beta waitlist.'
    })
  }, 1000)
}
</script>

<template>
  <div v-if="page">
    <UPageHero
      :title="page.title"
      :description="page.description"
    >
      <template #top>
        <HeroBackground />
      </template>

      <template #title>
        <MDC
          :value="page.title"
          unwrap="p"
        />
      </template>

      <PromotionalVideo />
    </UPageHero>

    <UPageSection
      v-for="(section, index) in page.sections"
      :key="index"
      :title="section.title"
      :description="section.description"
      :orientation="section.orientation"
      :reverse="section.reverse"
      :features="section.features"
    >
      <img
        v-if="section.image && section.image.src"
        :src="section.image.src"
        :alt="section.image.alt"
        class="w-full rounded-md shadow-xl ring-1 ring-gray-300 dark:ring-gray-700"
      />
      <ImagePlaceholder v-else />
    </UPageSection>

    <UPageSection
      :title="page.features.title"
      :description="page.features.description"
    >
      <UPageGrid>
        <UPageCard
          v-for="(item, index) in page.features.items"
          :key="index"
          v-bind="item"
          spotlight
        />
      </UPageGrid>
    </UPageSection>

    <USeparator />

    <UPageCTA
      v-bind="page.cta"
      variant="naked"
      class="overflow-hidden"
    >
      <div>To ensure every pilot deployment receives our full attention, we are approving access on a rolling basis. By registering now, you ensure your organization is vetted and inserted into the program at the earliest available window.</div>
      <template #links>
        <form
          class="flex flex-col gap-y-3 w-full max-w-sm"
          @submit.prevent="onSubmit"
        >
          <UFormField
            name="email"
            label=""
            size="lg"
          >
            <UInput
              v-model="email"
              type="email"
              class="w-full"
              placeholder="Enter your email"
              :loading="loading"
            >
              <template #trailing>
                <UButton
                  type="submit"
                  size="xs"
                  color="primary"
                  label="Join"
                  :loading="loading"
                />
              </template>
            </UInput>
          </UFormField>
        </form>
      </template>
      <LazyStarsBg />
    </UPageCTA>
  </div>
</template>
