<template>
  <q-page class="flex flex-center column bg-green-3">

    <h2> Добро пожаловать</h2>
    <q-img src="http://pm1.narvii.com/7845/fa846a0fd43025b6079f3d2f18196fc55381625ar1-955-955v2_hq.jpg" width="30%" class="main_Image"/>

    <p class="text-bold">
      для перехода к списку задач воспользуйтесь
      <router-link to="/catalog" class="text-h5">ссылкой</router-link>
    </p>

  </q-page>
</template>

<script setup>

</script>
<style>
.main_Image{
  border-radius: 2%;
}
</style>
