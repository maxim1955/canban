<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar class="bg-green-5 text-white shadow-2 rounded-borders flex justify-evenly ">
        <router-link to="/" class="nav_link">CanbanSystem</router-link>
        <router-link to="/catalog" class="nav_link">Каталог</router-link>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <router-view/>
    </q-page-container>

    <q-footer elevated class="bg-grey-8 text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          <div>Title</div>
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>

  </q-layout>
</template>
<style>
.nav_link{
  font-size: 30px;
  text-decoration: none;
  color: #31CCEC;
}
.nav_link:hover{
  text-decoration: underline;
}
</style>
<script setup>
</script>
