<template>
  <div class="ma-3">
    <v-card class="light-blue darken-3">
      <h3 class="text-h4 pa-3 text-center">Witaj na przykładowym teście z Technologii Sieci Web</h3>
      <v-divider></v-divider>
      <div>
        <h4 class="text-h4 pa-3 text-center">Instrukcja</h4>
        <div>
          <ul class="browser-default text-h5 mx-5">
            <li>Ns test przeznaczone jest 60 minut</li>
            <li>Test składa się z 30 pytań, za każde możesz otrzymać 1 punkt</li>
            <li>Test jest w formie pytań ABCD wielokrotnego wyboru - każde pytanie może mieć zero, jedną, wiele lub wszystkie poprawne odpowiedzi</li>
            <li>Zaznacz symbolem ☑ tylko poprawne odpowiedzi</li>
            <li>Punktacja: 18 – 19pkt 3.0, 20 – 22pkt 3.5, 23 - 25pkt 4.0, 26 - 28pkt 4.5, 29 - 30pkt 5.0</li>
          </ul>
        </div>
        <v-divider></v-divider>
        <h4 class="text-h5 py-3 text-center">Powodzenia!</h4>
      </div>
    </v-card>
    <v-btn block rounded color="green darken-3 my-3" router :to="{ name: 'Test'}"
    :class="{'animated pulse slow infinite': animatedBtn}" @mouseover="animatedBtn = true" @mouseleave="animatedBtn = false">Rozpocznij test!</v-btn>
  </div>
</template>

<script>
export default {
  data() {
    return {animatedBtn: false};
  }
};
</script>