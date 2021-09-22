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
      const points = [...lines].splice(1);

      const totalRounds = points.length;

      const numberRegex = /^[0-9]*$/;

      if(!lines[0].match(numberRegex)) {
        this.error =
          'Caracteres invalidos en el número de rondas indicadas.';
        return;
      }

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

      const differenceRoundes = [];

      points.forEach(p => {
          let results = p.split(' ');
          
          if(!results[0].match(numberRegex) || !results[1].match(numberRegex)) {
            this.error =
              'Caracteres invalidos en los puntajes de las rondas.';
            return;
          }

          results = results.map(Number);
          const playerWinner = results.indexOf(Math.max(...results)) + 1;
          differenceRoundes.push([playerWinner, Math.abs(results[0] - results[1])]);

      });

      const maxPoint = differenceRoundes.map(e => Math.max.apply(null, e));
      const winner = differenceRoundes[maxPoint.indexOf(Math.max(...maxPoint))];

      this.finalContent = winner.join(' ');

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