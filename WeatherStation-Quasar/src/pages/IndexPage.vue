<template>
  <q-page class="flex flex-center">
    <div class="">
      <q-banner class="bg-secondary">
        <q-banner class="bg-primary">
          <q-banner class="bg-secondary text-white">
            <p><center><q-icon name="signal_wifi_off" color="info" /></center></p>
            <p><center>{{26}} {{ "° C" }}</center></p>
            <p><center>{{ "Temperature" }}</center></p>
          </q-banner>
        </q-banner>
        <q-banner class="bg-primary">
          <q-banner class="bg-secondary text-white">
            <p><center><q-icon name="signal_wifi_off" color="info" /></center></p>
            <p><center>{{26}} {{ "%" }}</center></p>
            <p><center>{{ "Humidity" }}</center></p>
          </q-banner>
        </q-banner>
      </q-banner>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
var temp = "° C";
var hum = "%";
export default defineComponent({
  name: 'IndexPage'
})
</script>
