<template>
<div id="mail">
  <h2>Problema 1</h2>
  <p>Sube el archivo para resolver problema 1.</p>

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
  name: "Problem1",
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

      if (lines.length < 3 || lines.length > 4) {
        this.error = "Formato de archivo incorrecto";
        return;
      }

      const numbers = lines[0].split(" ");
      const firstInstruction = lines[1];
      const secondInstruction = lines[2];
      const message = lines[3];

      if (!message.match(/^[a-zA-Z0-9_-]*$/)) {
        this.error = "Caracteres invalidos dentro del mensaje";
        return;
      }

      const m1 = +numbers[0]
      const m2 = +numbers[1];
      const n = +numbers[2];

      if (m1 < 2 || m1 > 50) {
        this.error = "M1 no esta entre el rango de 2 a 50.";
        return;
      }

      if (m2 < 2 || m2 > 50) {
        this.error = "M2 no esta entre el rango de 2 a 50.";
        return;
      }

      if (n < 3 || n > 5000) {
        this.error = "N no esta entre el rango de 3 a 5000.";
        return;
      }

      if(firstInstruction.length !== m1) {
        this.error = `La primera instrucción no cuenta con el número de caracteres indicado: ${m1}`;
        return;
      }

      if(secondInstruction.length !== m2) {
        this.error = `La segunda instrucción no cuenta con el número de caracteres indicado: ${m2}`;
        return;
      }

      if(message.length !== n) {
        this.error = `El mensaje no cuenta con el número de caracteres indicado: ${n}`;
        return;
      }

      const messageHasError = message.split("").some((v, i, a) => {
        return v === a[i++];
      });

      let finalMessage = message;

      const ejemplo = Array.from(message);

      if (messageHasError) {
        const messageArray = [];
        ejemplo.forEach((a, i) => {
          if (this._checkLetter(ejemplo, a, i)) {
            messageArray.push(a);
          }
        });
        finalMessage = messageArray.join("");
      }

      if (finalMessage.includes(firstInstruction)) {
        this.finalContent += "SI\n";
      } else {
        this.finalContent += "NO\n";
      }

      if (finalMessage.includes(secondInstruction)) {
        this.finalContent += "SI\n";
      } else {
        this.finalContent += "NO\n";
      }

      this._generateFileProblem1();
    },
    _checkLetter(array, m, i) {
      const newI = i + 1;
      if (m !== array[newI] ||
          ((m.toLowerCase() === 'r' || m.toLowerCase() === 'l' ) &&
            m === array[newI] && ['a','e','i','o','u'].includes(array[newI+1]) && ['a','e','i','o','u'].includes(array[i - 1]))) {

          return true;
      }
        this._checkLetter([...array], m, newI);
    },
    _generateFileProblem1() {
      const myblob = new Blob([this.finalContent], {
        type: "text/plain",
      });
      const a = document.createElement("a");
      a.href = window.URL.createObjectURL(myblob);
      a.download = `resultProblem1.txt`;
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