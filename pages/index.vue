<template>
  <div
    id="wrapper"
    class="container mx-auto min-h-screen flex items-center justify-center"
  >
    <div
      v-if="loading"
      class="p-12 rounded bg-black bg-opacity-25 flex flex-col items-center justify-center fixed text-center text-white font-bold"
      style="backdrop-filter: blur(5px); text-shadow: 2px 2px 4px #000;"
    >
      Gerando o seu certificado,<br />
      por favor aguarde um instante...
      <i class="gg-spinner mt-8"></i>
    </div>
    <main class="flex flex-col md:flex-row items-center">
      <div class="w-full md:w-3/5 px-8 md:px-0">
        <div
          class="text-white font-bold"
          style="text-shadow: 2px 2px 4px #000;"
        >
          <header
            class="bg-black bg-opacity-25 flex justify-center rounded p-4 mb-6"
            style="backdrop-filter: blur(5px);"
          >
            <img src="/logo.png" alt="" />
          </header>
          <p>Olá !</p>
          <p class="my-2 md:text-xl">
            Preencha seu nome completo no campo abaixo para fazer o download de
            seu certificado de participação da FORMAÇÃO EM INTELIGÊNCIA
            EMOCIONAL - ministrado pelo
            <br />Prof. Me. Marlos Ramos que aconteceu ao vivo entre os dias 07
            a 10 de setembro de 2020.
          </p>
          <p>
            Atenciosamente,<br />
            Marlos Ramos
          </p>
        </div>
        <footer>
          <form
            @submit.prevent="geraPDF"
            class="flex flex-col md:flex-row mt-8 items-end"
          >
            <label
              class="flex flex-col text-white w-full"
              style="text-shadow: 2px 2px 4px #000;"
            >
              Nome completo:
              <input
                type="text"
                class="px-4 py-2 text-black w-full shadow-lg"
                v-model="nome"
                autofocus
              />
            </label>
            <input
              type="submit"
              value="Baixar meu certificado"
              class="w-full md:w-auto shadow-lg px-4 py-2 bg-green-500 text-green-100 font-bold mt-2 md:mt-0 md:ml-4 cursor-pointer"
            />
          </form>
        </footer>
      </div>
      <div class="hidden md:flex md:w-2/5">
        <img
          src="https://blob.contato.io/machine-user-images/foto2-img-2125937-20200811170257.png"
          alt="Marlos Ramos"
        />
      </div>
    </main>
    <img id="bg" class="hidden" src="/certificado.png" alt="" />
  </div>
</template>

<script>
import { jsPDF } from "jspdf";
import { Bad } from "@/static/BadScript-Regular.ttf";

export default {
  data() {
    return {
      nome: "",
      loading: false
    };
  },
  methods: {
    async geraPDF() {
      this.loading = true;

      let nome = this.nome.toLowerCase().split(" ");
      for (let index = 0; index < nome.length; index++) {
        nome[index] =
          nome[index].charAt(0).toUpperCase() + nome[index].slice(1);
      }
      nome = nome
        .join(" ")
        .replace(/ Da /g, " da ")
        .replace(/ De /g, " de ")
        .replace(/ Dos /g, " dos ");

      this.nome = nome;

      const doc = new jsPDF({ orientation: "landscape" });

      doc.addFileToVFS("BadScript-Regular.ttf", Bad);
      doc.addFont("BadScript-Regular.ttf", "Bad", "normal");
      doc.setFont("Bad");
      doc.setFontSize(22);

      let bg = document.querySelector("#bg");
      doc.addImage(bg, "PNG", 0, 0, 297, 210);

      doc.text(nome, 297 / 2, 104, { align: "center" });

      doc.save(`${nome}.pdf`);
      let vm = this;
      setTimeout(() => {
        vm.loading = false;
      }, 5000);
    }
  }
};
</script>

<style>
body {
  background: url("https://blob.contato.io/machine-user-images/banner-fundo-lead-img-2125937-20200811160429.png")
    0% 0% / cover repeat scroll #1574a4;
}

.gg-spinner {
  transform: scale(var(--ggs, 1));
}

.gg-spinner,
.gg-spinner::after,
.gg-spinner::before {
  box-sizing: border-box;
  position: relative;
  display: block;
  width: 20px;
  height: 20px;
}

.gg-spinner::after,
.gg-spinner::before {
  content: "";
  position: absolute;
  border-radius: 100px;
}

.gg-spinner::before {
  animation: spinner 1s cubic-bezier(0.6, 0, 0.4, 1) infinite;
  border: 3px solid transparent;
  border-top-color: currentColor;
}

.gg-spinner::after {
  border: 3px solid;
  opacity: 0.2;
}

@keyframes spinner {
  0% {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}
</style>
