<script setup>
import { useI18n } from 'vue-i18n';
import SmartSvg from './SmartSvg.vue';
import { useSmartLinks } from '@/composables/smart-links';

const year = new Date().getFullYear();
const { t } = useI18n({ useScope: 'global' });

const { socialLinks, routeLinks } = useSmartLinks();
</script>

<template>
  <hr class="my-16" />
  <footer
    class="mx-auto mb-20 grid max-w-7xl grid-cols-2 gap-3 gap-y-10 px-6 text-sm sm:px-10 md:grid-cols-4 lg:justify-items-center lg:text-lg"
  >
    <div class="flex flex-col gap-3">
      <h5 class="mb-3 font-semibold">{{ t('footer.navigation') }}</h5>
      <div class="flex flex-col items-start gap-1 lg:text-base">
        <RouterLink
          v-for="link in routeLinks"
          :key="link.name"
          :to="link.to"
          class="hover:text-primary"
          active-class="text-primary underline underline-offset-2"
        >
          {{ link.name }}
        </RouterLink>
      </div>
    </div>
    <div class="flex flex-col gap-3">
      <h5 class="mb-3 font-semibold">{{ t('footer.information') }}</h5>
      <div class="flex flex-col items-start gap-1 lg:text-base">
        <a href="tel:9498060123" class="cursor-pointer">(949) 806-0123</a>
        <RouterLink to="/contact">
          {{ t('footer.careers') }}
        </RouterLink>
        <RouterLink to="/contact">Catering</RouterLink>
      </div>
    </div>
    <div class="flex flex-col gap-3 md:order-first">
      <h5 class="mb-3 font-semibold">Las Costas</h5>
      <div class="flex gap-4 text-base lg:text-lg">
        <a v-for="link in socialLinks" :key="link.name" :href="link.url" target="_blank">
          <SmartSvg :src="link.icon" />
        </a>
      </div>
      <!-- Had to do it this way not sure why -->
      <div class="flex grow items-end text-xs sm:w-4/5">
        <p>
          Copyright ©
          <span>{{ year }}</span>
          by Las Costas. All rights reserved.
        </p>
      </div>
    </div>
    <div class="flex flex-col gap-3 md:order-first">
      <h5 class="mb-3 font-semibold">{{ t('footer.business_hours') }}</h5>
      <p class="sm:w-4/5 lg:text-base">
        31345 Los Rios St, San Juan Capistrano, CA 92675
      </p>
      <div class="lg:text-base">
        <p>{{ t('footer.schedule') }}</p>
      </div>
    </div>
  </footer>
</template>
