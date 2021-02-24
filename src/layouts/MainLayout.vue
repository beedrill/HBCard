<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="bg-grey-5 ">
            <img class="q-mx-sm" src="~assets/Icon.png">
        <q-toolbar-title class="text-grey-9">
          名片生产器
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MainLayout',
  components: {},
  data () {
    return {
    }
  }
}
</script>
