<template>
  <div>
    <v-card class="light-grey darken-3 pa-2">
      <v-form v-model="formState">
        <div class="px-5">
          <v-text-field
            clearable
            :rules="[rules.required]"
            color="green"
            label="Pytanie"
            v-model="Question"
          ></v-text-field>
          <v-text-field
            clearable
            color="green"
            label="Pole z kodem (opcjonalne)"
            hint="Zwiń kod do jednej linii i zamień znaki końca linii na \n"
            v-model="Textarea"
          ></v-text-field>
          <v-divider></v-divider>
          <h3 block large class="ma-2 text-center grey--text">(Opcjonalne) Dodaj obrazek</h3>
          <div>
            <v-container class="pa-0">
              <v-layout row wrap justify-space-between>
                <v-flex xs12 md6 class="px-2">
                  <input
                    style="display: none"
                    type="file"
                    accept="image/*"
                    @change="onFileSelected"
                    ref="fileInput"
                  />
                  <v-btn block rounded color="green" @click="$refs.fileInput.click()">
                    <span>Wybierz obrazek</span>
                    <v-icon right>mdi-image-plus</v-icon>
                  </v-btn>
                </v-flex>
                <v-flex xs12 md6 class="px-2">
                  <v-btn
                    block
                    rounded
                    color="green"
                    @click="uploadImage"
                    :disabled="selectedFile === ''"
                  >
                    <span>Wgraj plik</span>
                    <v-icon right>mdi-cloud-upload</v-icon>
                  </v-btn>
                </v-flex>
              </v-layout>
            </v-container>
            <v-text-field readonly color="green" label="Plik" v-model="selectedFile.name"></v-text-field>
            <v-progress-linear v-if="selectedFile" color="success" v-model="uploadState"></v-progress-linear>
            <v-btn block rounded color="red" v-if="Image" @click="Image = ''">
              <span>Usuń obrazek</span>
              <v-icon right>mdi-delete</v-icon>
            </v-btn>
          </div>
          <v-divider></v-divider>
          <h2 block large class="mt-3 text-center">Zaznacz, które odpowiedzi są prawidłowe</h2>
          <div class="pa-0">
            <v-container class="pa-0">
              <v-flex xs12>
                <v-layout>
                  <v-flex xs1>
                    <v-checkbox color="green" v-model="GoodAns[0]"></v-checkbox>
                  </v-flex>
                  <v-flex xs11>
                    <v-text-field
                      clearable
                      :rules="[rules.required]"
                      color="green"
                      label="Odpowiedź A"
                      v-model="Ans1"
                    ></v-text-field>
                  </v-flex>
                </v-layout>
              </v-flex>
              <v-flex xs12>
                <v-layout>
                  <v-flex xs1>
                    <v-checkbox color="green" v-model="GoodAns[1]"></v-checkbox>
                  </v-flex>
                  <v-flex xs11>
                    <v-text-field
                      clearable
                      :rules="[rules.required]"
                      color="green"
                      label="Odpowiedź B"
                      v-model="Ans2"
                    ></v-text-field>
                  </v-flex>
                </v-layout>
              </v-flex>
              <v-flex xs12>
                <v-layout>
                  <v-flex xs1>
                    <v-checkbox color="green" v-model="GoodAns[2]"></v-checkbox>
                  </v-flex>
                  <v-flex xs11>
                    <v-text-field
                      clearable
                      :rules="[rules.required]"
                      color="green"
                      label="Odpowiedź C"
                      v-model="Ans3"
                    ></v-text-field>
                  </v-flex>
                </v-layout>
              </v-flex>
              <v-flex xs12>
                <v-layout>
                  <v-flex xs1>
                    <v-checkbox color="green" v-model="GoodAns[3]"></v-checkbox>
                  </v-flex>
                  <v-flex xs11>
                    <v-text-field
                      clearable
                      :rules="[rules.required]"
                      color="green"
                      label="Odpowiedź D"
                      v-model="Ans4"
                    ></v-text-field>
                  </v-flex>
                </v-layout>
              </v-flex>
            </v-container>
          </div>
        </div>
      </v-form>
      <v-btn block rounded color="red" @click="clearAddForm()">
        <span>Wyczyść formularz</span>
        <v-icon right>mdi-delete</v-icon>
      </v-btn>
    </v-card>
    <h2 block large class="ma-3 text-center">Podgląd pytania</h2>
    <!-- Podgląd pytania -->
    <div class="mb-4">
      <v-card class="light-grey darken-3 mb-2">
        <v-card-title class="pa-3">
          <div>
            <h3 class="text-h5 mb-0">Pytanie [numer]</h3>
            <div>{{Question}}</div>
          </div>
        </v-card-title>
        <div class="ma-2">
          <div v-if="Textarea != ''" class="pa-2" style="border: 1px dashed;">
            <p
              class="ma-0 pa-0"
              v-for="(row, index) in prepareTextArea(Textarea)"
              :key="index"
            >{{row}}</p>
          </div>
          <div v-if="Image != ''">
            <img :src="Image" />
          </div>
        </div>
        <v-divider></v-divider>
        <div class="pa-3">
          <v-label>Odpowiedź A: {{Ans1}}</v-label>
          <v-divider></v-divider>
          <v-label>Odpowiedź B: {{Ans2}}</v-label>
          <v-divider></v-divider>
          <v-label>Odpowiedź C: {{Ans3}}</v-label>
          <v-divider></v-divider>
          <v-label>Odpowiedź D: {{Ans4}}</v-label>
        </div>
      </v-card>
    </div>
    <v-container class="pa-0">
      <v-layout row wrap justify-space-between>
        <v-flex xs12 md6 class="px-2">
          <v-btn
            block
            :disabled="!formState || !Question || !Ans1 || !Ans2 || !Ans3 || !Ans4"
            rounded
            color="green darken-3"
            @click="acceptQuestion()"
          >
            <span>{{buttons.acceptText}}</span>
            <v-icon right>{{buttons.acceptIcon}}</v-icon>
          </v-btn>
        </v-flex>
        <v-flex xs12 md6 class="px-2">
          <v-btn block rounded color="red" @click="cancelEdit()">
            <span>{{ buttons.cancelText }}</span>
            <v-icon right>mdi-cancel</v-icon>
          </v-btn>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
/* eslint-disable no-console */
import axios from "../../js/axiosData";
import firebase from "../../firebase/init";
let db = firebase.firestore();
export default {
  props: ["buttons", "preload"],
  data() {
    return {
      Question: "",
      Ans1: "",
      Ans2: "",
      Ans3: "",
      Ans4: "",
      GoodAns: [false, false, false, false],
      Textarea: "",
      Image: "",
      selectedFile: "",
      uploadState: 0,
      formState: false,
      rules: {
        required: value => !!value || "Pole wymagane"
      }
    };
  },
  created() {
    if (this.preload === "yes") {
      axios
        .get("Questions/" + this.$route.params.questionId)
        .then(data => {
          console.log(data);
          const question = data.data.fields;
          this.Question = question.Question.stringValue;
          this.Ans1 = question.Ans1.stringValue;
          this.Ans2 = question.Ans2.stringValue;
          this.Ans3 = question.Ans3.stringValue;
          this.Ans4 = question.Ans4.stringValue;
          this.GoodAns = [
            question.GoodAns.arrayValue.values[0].booleanValue,
            question.GoodAns.arrayValue.values[1].booleanValue,
            question.GoodAns.arrayValue.values[2].booleanValue,
            question.GoodAns.arrayValue.values[3].booleanValue
          ];
          this.Textarea = question.Textarea.stringValue;
          this.Image = question.Image.stringValue;
        })
        .catch(err => {
          console.log(err);
          this.$store.dispatch("modifySnackbar", {
            state: true,
            msg: "Wystąpił błąd przy wczytywaniu pytania pytania",
            color: "error"
          });
        });
    }
  },
  methods: {
    prepareTextArea(text) {
      return text.split("\\n");
    },
    clearAddForm() {
      this.Question = "";
      this.Ans1 = "";
      this.Ans2 = "";
      this.Ans3 = "";
      this.Ans4 = "";
      this.GoodAns = [false, false, false, false];
      this.Textarea = "";
      this.Image = "";
      this.selectedFile = "";
      this.uploadState = 0;
    },
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
    },
    uploadImage() {
      let storage = firebase.storage();
      let img = String(Date.now()) + "_" + this.selectedFile.name;
      let task = storage.ref("images/" + img).put(this.selectedFile);
      let vm = this;
      task.on(
        "state_changed",
        function(snapshot) {
          let precentage =
            (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
          vm.uploadState = precentage;
        },
        function error(e) {
          throw console.error(e);
        },
        function() {
          task.snapshot.ref.getDownloadURL().then(downloadURL => {
            vm.Image = downloadURL;
          });
        }
      );
    },
    acceptQuestion() {
      this.$emit("acceptQuestion", {
        Question: this.Question,
        Ans1: this.Ans1,
        Ans2: this.Ans2,
        Ans3: this.Ans3,
        Ans4: this.Ans4,
        GoodAns: this.GoodAns,
        Textarea: this.Textarea,
        Image: this.Image
      });
    },
    cancelEdit() {
      this.$router.push({ name: "Admin" });
    }
  }
};
</script>