<script setup>
import { ref, watch } from 'vue';
import { useRoute } from 'vue-router';
import { useI18n } from 'vue-i18n';
import { useWindowScroll, useWindowSize } from '@vueuse/core';
import SmartTransition from '@/components/smart/SmartTransition.vue';
import LocaleSelector from '@/components/LocaleSelector.vue';
import SmartSvg from './SmartSvg.vue';
import { useSmartLinks } from '@/composables/smart-links';

const { t } = useI18n({ useScope: 'global' });

const showOffCanvas = ref(false);
const route = useRoute();

const { socialLinks, routeLinks } = useSmartLinks();
const { width: windowWidth } = useWindowSize();
const { y: scrollY } = useWindowScroll();

watch(windowWidth, () => {
  if (windowWidth.value >= 1024) {
    showOffCanvas.value = false;
  }
});

watch(route, () => {
  showOffCanvas.value = false;
});

watch(showOffCanvas, (value) => {
  document.body.style.overflow = value ? 'hidden' : 'visible';
});
</script>
<template>
  <div
    class="bg-primary-50 sticky inset-x-0 top-0 z-40 mx-auto flex h-[60px] content-center items-center justify-between px-4 transition-all lg:h-[68px] lg:px-8 xl:px-12"
    :class="{ 'shadow-md': scrollY > 0 }"
  >
    <RouterLink class="lg:ml-7" to="/">
      <img
        src="/andariego-nav.png"
        alt="logo"
        class="h-[68px] w-[68px] lg:h-[78px] lg:w-[76px]"
      />
    </RouterLink>
    <div
      class="invisible hidden text-lg font-semibold lg:visible lg:flex lg:content-center lg:items-center lg:gap-8"
    >
      <RouterLink
        v-for="link in routeLinks"
        :key="link.name"
        :to="link.to"
        class="hover:text-primary"
        active-class="text-primary underline underline-offset-2"
      >
        {{ link.name }}
      </RouterLink>

      <a href="tel:9498060123" class="btn btn-primary">
        {{ t('link.call') }}
      </a>
    </div>
    <button class="lg:hidden!" type="button" @click="showOffCanvas = !showOffCanvas">
      <SmartTransition mode="out-in">
        <SmartSvg v-if="showOffCanvas" src="x" />
        <SmartSvg v-else src="bars" />
      </SmartTransition>
    </button>
    <Teleport to="body">
      <SmartTransition name="slide-from-right">
        <div
          v-if="showOffCanvas"
          class="bg-primary-50 fixed bottom-0 left-0 top-[60px] z-40 h-screen w-screen overflow-scroll"
        >
          <div class="mt-12 flex flex-col items-center gap-9 text-center font-semibold">
            <RouterLink
              v-for="link in routeLinks"
              :key="link.name"
              :to="link.to"
              class="text-3xl hover:text-primary"
              active-class="text-primary underline underline-offset-2"
            >
              {{ link.name }}
            </RouterLink>

            <div class="flex justify-center gap-6 text-3xl">
              <a
                v-for="link in socialLinks"
                :key="link.name"
                :href="link.url"
                target="_blank"
              >
                <SmartSvg :src="link.icon" />
              </a>
            </div>
            <a href="tel:9498060123" class="btn btn-primary btn-lg mb-3 text-3xl">
              {{ t('link.call') }}
            </a>
            <LocaleSelector class="-ms-1 text-xl" />
          </div>
        </div>
      </SmartTransition>
    </Teleport>
  </div>
</template>

<style lang="scss" scoped>
a {
  transition: all 150ms linear;
}

a:hover {
  color: #71706a;
}
</style>
