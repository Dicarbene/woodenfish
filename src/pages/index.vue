<script setup lang="ts">
const user = useUserStore()
const name = $ref(user.savedName)
const { t, availableLocales, locale } = useI18n()
const router = useRouter()
const go = () => {
  if (name)
    router.push(`/hi/${encodeURIComponent(name)}`)
}
const toggleLocales = () => {
  // change to some real logic
  const locales = availableLocales
  locale.value = locales[(locales.indexOf(locale.value) + 1) % locales.length]
}
</script>

<template>
  <nav p-2em w-full text-base display-none md:display-block>
    <div class="right" text-lg flex justify-center gap-4 style="font-size: 2em">
      <button class="icon-btn  !outline-none" :title="t('button.toggle_dark')" @click="toggleDark()">
        <div i="line-md-sunny-outline dark:line-md-moon" />
      </button>
      <a class="icon-btn " :title="t('button.toggle_langs')" @click="toggleLocales()">
        <div i-carbon-language />
      </a>
      <a class="icon-btn " rel="noreferrer" href="https://github.com/dicarbene" target="_blank" title="GitHub">
        <div i-line-md-github-loop />
      </a>
    </div>
  </nav>
  <WoodenFish />
</template>
