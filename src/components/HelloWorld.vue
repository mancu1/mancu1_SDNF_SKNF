<template>
  <v-container>
    <v-row>
      <v-col>
        <h2>Доброго времени суток от Мансура</h2>
        <br>
        <h1>Ключ К = 4172536</h1></v-col>
    </v-row>
    <v-row class="d-flex row mx-auto my-2">
<!--      <v-btn @click="first">-->
<!--        Посчитать f1() = 00001111-->
<!--      </v-btn>-->
<!--      <v-btn @click="second">-->
<!--        Посчитать f2() = 11110001-->
<!--      </v-btn>-->
    </v-row>
    <v-row class="d-flex row mx-auto my-2">
      <v-col cols="8">
        <v-text-field type="text" v-model="input"/>
      </v-col>
      <v-col cols="2">
        <v-btn @click="cript">
          Защифровать
        </v-btn>
      </v-col>
      <v-col cols="2">
        <v-btn @click="decript">
          Дещифровать
        </v-btn>
      </v-col>
<!--      <v-btn @click="download">-->
<!--        Скачать-->
<!--      </v-btn>-->
    </v-row>
    <h3>{{spliting}}</h3>
    <h3>{{cripted}}</h3>
    <h3>{{decripted}}</h3>
  </v-container>
</template>

<script>
import _ from "lodash";
export default {
  name: "HelloWorld",
  data() {
    return {
      maping: "4172536",
      input: "",
      spliting: [],
      cripted: "",
      decripted: ""
    };
  },
  computed: {
    arrMaping() {
      return this.maping.split("")
    },
  },
  methods: {
    cript() {
      this.culc();
      const newSplited = this.spliting.map(el => {
        return this.arrMaping.map(key => el[Number(key)-1])
      })
      this.cripted = newSplited.map(str => str.join("")).join("-");
    },
    decript() {
      this.culc();
      console.log(this.spliting);
      const newSplited = this.spliting.map(el => {
        return el.map((simbol, index) => el[this.arrMaping.findIndex(key => Number(key)-1 === index)])
      })
      this.decripted = newSplited.map(str => str.join("")).join("-");
    },
    culc() {
      let workInput = this.input;
      const stringLen = this.input.length;
      const isSeven = stringLen%7;
      if (!(isSeven === 0)) {
        for (let i = 0; i < (7- isSeven); i++) {
          workInput+="*";
        }
      }
      const allSimbols = workInput.split("");
      console.log({ workInput, stringLen, isSeven, allSimbols });
      this.spliting = _.chunk(allSimbols, 7);
    },
    download: function() {
      if (this.input) {
        this.culc();
        let element = document.createElement("a");
        element.setAttribute(
          "href",
          "data:text/plain;charset=utf-8," +
            encodeURIComponent(
              `f()=${this.input} \n${this.formattedSDNF} \n${this.formattedSKNF}`
            )
        );
        element.setAttribute("download", "mancu1.txt");

        element.style.display = "none";
        document.body.appendChild(element);

        element.click();

        document.body.removeChild(element);

        window.cl;
      }
    }
  }
};
</script>
