<template>
  <div class="ma-3">
    <v-card class="light-blue darken-3">
      <h3 class="text-h3 py-3 text-center text-uppercase">Wynik testu</h3>
      <v-divider></v-divider>
      <div class="mx-2 my-0">
        <v-alert prominent class="text-h5 text-center my-1" type="info">
          Udało Ci się uzyskać
          <b>{{points}} punkt(ów)</b> z testu.
        </v-alert>
        <v-alert
          prominent
          class="text-h5 text-center my-1"
          :type="points >= 18 ? 'success' : 'error'"
        >Gdyby to był egzamin, otrzymał(a) byś ocenę {{mark}}</v-alert>
      </div>
      <v-divider></v-divider>
      <div>
        <v-container class="pa-1 px-3">
          <v-layout row wrap justify-space-between>
            <v-flex xs12 md6 class="my-1 px-2">
              <v-btn
                large
                block
                rounded
                class="green"
                router
                :to="{name: 'Index'}"
              >Powrót na stronę główną</v-btn>
            </v-flex>
            <v-flex xs12 md6 class="my-1 px-2">
              <v-btn
                large
                block
                rounded
                class="green"
                router
                :to="{name: 'Test'}"
              >Rozpocznij kolejny Test</v-btn>
            </v-flex>
          </v-layout>
        </v-container>
      </div>
    </v-card>
    <ResultQuestions :Questions="Questions" :Answers="Answers" />
  </div>
</template>

<script>
const ResultQuestions = () =>
  import(/* webpackChunkName: "ResultQuestions" */ "./ResultQuestions");
export default {
  props: ["points", "Questions", "Answers"],
  components: {
    ResultQuestions
  },
  data() {
    return {
      mark: null,
      color: null
    };
  },
  methods: {
    getMark(points) {
      if (points < 18) {
        return { mark: "2.0", color: "red" };
      } else if (points < 20) {
        return { mark: "3.0", color: "orange" };
      } else if (points < 23) {
        return { mark: "3.5", color: "amber" };
      } else if (points < 26) {
        return { mark: "4.0", color: "yellow darken-2" };
      } else if (points < 29) {
        return { mark: "4.5", color: "light-green" };
      } else {
        return { mark: "5.0", color: "green" };
      }
    }
  },
  mounted() {
    let res = this.getMark(this.points);
    this.mark = res.mark;
    this.color = res.color + " text-h5 text-center";
  }
};
</script>