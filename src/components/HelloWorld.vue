<template>
  <v-container>
    <v-row>
      <h2>Доброго времени суток от Мансура</h2>
    </v-row>
    <v-row class="d-flex row mx-auto my-2">
      <v-btn @click="first">
        Посчитать f1() = 00001111
      </v-btn>
      <v-btn @click="second">
        Посчитать f2() = 11110001
      </v-btn>
    </v-row>
    <v-row class="d-flex row mx-auto my-2">
      <v-text-field type="text" v-model="input" />
      <v-btn @click="culc">
        Посчитать свое
      </v-btn>
      <v-btn @click="download">
        Скачать
      </v-btn>
    </v-row>
    <v-card>
      <div>
        <div v-for="(line, index) in truthTable" :key="index">
          <div
            style="
    display: flex;
    flex-direction: row;"
          >
            <p style="margin: 3px;" v-for="(sel, ind) in line" :key="ind">
              {{ sel }}
            </p>
          </div>
        </div>
      </div>
      <p>
        {{ formattedSDNF }}
      </p>
      <p>
        {{ formattedSKNF }}
      </p>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      input: "",
      truthTable: [],
      SDNF: "",
      SKNF: "",
      symbols: ["x", "y", "z", "a", "b", "c"]
    };
  },
  computed: {
    formattedSDNF: {
      get() {
        return this.SDNF ? "СДНФ = " + this.SDNF : "";
      }
    },
    formattedSKNF: {
      get() {
        return this.SKNF ? "СКНФ = " + this.SKNF : "";
      }
    },

    numberOfColums: {
      get() {
        return Math.log(this.input.length) / Math.log(2); // ВВОДИМ КОЛ-ВА ПЕРЕМЕННЫХ И, ОДНОВРЕМЕННО, КОЛ-ВА СТОЛБЦОВ
      }
    },
    numberOfLines: {
      get() {
        return Math.pow(2, this.numberOfColums); // КОЛ-ВО СТРОК В МАТРИЦЕ
      }
    }
  },
  methods: {
    first: function() {
      this.input = "00001111";
      this.culc();
    },
    second: function() {
      this.input = "11110001";
      this.culc();
    },
    culc: function() {
      let test = [];
      test[0] = [];
      for (let a = 0; a <= this.numberOfColums; a++) {
        if (a < this.numberOfColums) {
          test[0][a] = this.symbols[a];
        } else {
          test[0][a] = "f()";
        }
      }
      for (let j = 0; j < this.numberOfLines; j++) {
        test[j + 1] = [];
        for (let i = 0; i <= this.numberOfColums; i++) {
          if (i !== this.numberOfColums) {
            if (
              (j + 1) % Math.pow(2, this.numberOfColums - i) <=
                Math.pow(2, this.numberOfColums - i) / 2 &&
              (j + 1) % Math.pow(2, this.numberOfColums - i) !== 0
            ) {
              test[j + 1][i] = "0";
            } else {
              test[j + 1][i] = "1";
            }
          } else {
            test[j + 1][i] = this.input.charAt(j);
          }
        }
      }
      this.truthTable = test;
      this.SDNF = this.getSDNF();
      this.SKNF = this.getSKNF();
    },
    getSDNF: function() {
      let res = "";
      let funcValues = this.input.split("");
      for (let i = 0; i < funcValues.length; i++) {
        for (let j = 0; funcValues[i] === "1" && j < this.numberOfColums; j++) {
          res +=
            this.truthTable[i][j] === "0"
              ? "!" + this.symbols[j]
              : this.symbols[j];
          if (j === this.numberOfColums - 1) {
            res += " | ";
          }
        }
      }
      return res.length === 0 ? res : res.substring(0, res.length - 3);
    },
    getSKNF: function() {
      let res = "";
      let funcValues = this.input.split("");
      let k = 1;
      for (let i = 0; i < funcValues.length; i++) {
        for (let j = 0; funcValues[i] === "0" && j < this.numberOfColums; j++) {
          if (k === 1) {
            res += "(";
            k = 0;
          }
          res +=
            this.truthTable[i][j] === "1"
              ? "!" + this.symbols[j]
              : this.symbols[j];
          if (j === this.numberOfColums - 1) {
            res += ") & ";
          } else {
            res += " | ";
          }
        }
        k = 1;
      }
      return res.length === 0 ? res : res.substring(0, res.length - 3);
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
