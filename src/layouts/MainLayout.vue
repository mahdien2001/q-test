<template>
  <q-layout view="lHh Lpr lFf">
    <!-- toolbar -->
    <toolbar></toolbar>

    <!-- lodeing -->
    <loading></loading>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>

import toolbar from "../components/toolbar.vue"
import loading from "../components/loading.vue"


export default {
  name: 'MainLayout',
  components: {
    toolbar,
    loading
  },
  data() {
    return {
      leftDrawerOpen: false,
    }
  }
}
</script>
