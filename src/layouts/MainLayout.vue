<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-indigo">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title> Webtronics </q-toolbar-title>
      </q-toolbar>
      <BreadCrumbs />
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> App Links </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
          :class="{ 'active-link': link.link === currentPath }"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import EssentialLink, {
  EssentialLinkProps,
} from 'components/EssentialLink.vue';
import BreadCrumbs from 'src/components/BreadCrumbs.vue';

const essentialLinks: EssentialLinkProps[] = [
  {
    title: 'Main',
    icon: 'school',
    link: '/',
  },
  {
    title: 'Tickets',
    icon: 'chat',
    link: '/tickets',
  },
  {
    title: 'Profile',
    icon: 'record_voice_over',
    link: '/profile',
  },
  {
    title: 'Login',
    icon: 'public',
    link: '/login',
  },
];
const currentPath = ref(window.location.pathname);
const leftDrawerOpen = ref(false);
// const currentRoute = computed(() => {
//   return essentialLinks;
// });

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>
