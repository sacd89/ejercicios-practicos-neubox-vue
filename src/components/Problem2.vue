<template>
<div id="main">
  <h2>Problema 2</h2>
  <p>Sube el archivo para resolver problema 2.</p>

  <input ref="upload" type="file" name="file-upload" @change="onUploadFiles" />

  <button class="btn btn-outline-secondary" v-if="fileSelected" @click="resolveProblem()">Resolver</button>

  <div v-if="finalContent && !error" class="alert alert-success" role="alert">
    {{ finalContent }}
  </div>
  <div v-if="error" class="alert alert-danger" role="alert">
    {{ error }}
  </div>
</div>
</template>

<script>
export default {
  name: "Problem2",
  data() {
    return {
      error: null,
      dataFile: null,
      fileSelected: null,
      finalContent: "",
    };
  },
  methods: {
    onUploadFiles(event) {
      this.error = null;
      this.finalContent = "";
      this.fileSelected = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => (this.dataFile = e.target.result);
      reader.readAsText(this.fileSelected);
    },
    resolveProblem() {
      const lines = this.dataFile.split("\n");
      const roundsNumber = +lines[0];

      const totalRounds = lines.length - 1;

      if (roundsNumber < 0 || roundsNumber > 10000) {
        this.error =
          "El número de rondas debe de ser mayor a 0 y menor o igual a 10000.";
        return;
      }

      if (totalRounds !== roundsNumber) {
        this.error =
          "El número de rondas encontrado no coincide con los ingresados en el archivo.";
        return;
      }

      const pointsPlayer1 = [];
      const pointsPlayer2 = [];

      for (let i = 1; i <= totalRounds; i++) {
        const points = lines[i].split(" ");
        const player1 = +points[0];
        const player2 = +points[1];

        const winnerPoint = Math.max(...points);

        let diffence = 0;

        if (winnerPoint === player1) {
          diffence = player1 - player2;
          pointsPlayer1.push(diffence);
        } else {
          diffence = player2 - player1;
          pointsPlayer2.push(diffence);
        }
      }

      const maxPointPlayer1 = Math.max(...pointsPlayer1);
      const maxPointPlayer2 = Math.max(...pointsPlayer2);

      if (maxPointPlayer1 > maxPointPlayer2) {
        this.finalContent = `1 ${maxPointPlayer1}`;
      } else {
        this.finalContent = `2 ${maxPointPlayer2}`;
      }

      this._generateFileProblem2();
    },
    _generateFileProblem2() {
      const myblob = new Blob([this.finalContent], {
        type: "text/plain",
      });
      const a = document.createElement("a");
      a.href = window.URL.createObjectURL(myblob);
      a.download = `resultProblem2.txt`;
      a.click();
    },
  },
};
</script>
<style>
 .alert{
   margin-top: 15px;
 }
  #main {
   margin-top: 15px;
 }
</style>