<template>
  <div class="container-chat">
    <div class="conversation" ref="scroll">
      <div v-for="(conversa, index) in conversation" :key="index" :class="{'admin': conversa.admin, 'usuario': !conversa.admin}">
        <v-card class="mx-auto card" :color="conversa.admin ? '#0F3460' : '#FBBB08'" dark width="400">
          <v-card-title>
            <v-list-item-avatar color="grey darken-3">
              <v-img class="elevation-6" alt="Imagem avatar" :src="conversa.avatar"></v-img>
            </v-list-item-avatar>
            <span class="text-h6 font-weight-light">{{ conversa.nome }}</span>
          </v-card-title>

          <v-card-text class="text-h2 font-weight-bold msg">
            <span>{{ conversa.msg }}</span>
          </v-card-text>

          <v-card-actions>
            <v-list-item>
              <v-row class="right" justify="end">
                <span class="subheading">{{ conversa.data }}</span>
              </v-row>
            </v-list-item>
          </v-card-actions>
        </v-card>
      </div>
    </div>
    <v-container>
      <v-row>
        <v-col cols="12">
          <v-text-field
            v-model="message"
            outlined
            :disabled="disabled"
            clearable
            label="Mensagem"
            v-on:keyup.enter="onEnter"
            type="text"
            id="inputEnviar"
            class="text-flex"
          >
            <template v-slot:append>
              <v-fade-transition leave-absolute>
                <v-progress-circular
                  v-if="loading"
                  size="24"
                  color="info"
                  indeterminate
                ></v-progress-circular>
                <v-icon
                  v-else
                  width="24"
                  height="24"
                  color="green darken-2"
                  v-on:click="send()"
                >
                  mdi-send
                </v-icon>
              </v-fade-transition>
            </template>
          </v-text-field>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "",
      loading: false,
      conversation: [],
      disabled: false,
      nomeAssistenteVirtual: "Olivia",
      ultimaPergunta: "",
      podeEscrever: true,
      avatarWoman: "https://avataaars.io",
      avatarBot:
        "https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light",
    };
  },
  mounted() {
    this.adicionarChatBox(
      true,
      this.avatarWoman,
      this.nomeAssistenteVirtual,
      `Olá! Eu sou a ${this.nomeAssistenteVirtual}, serei a sua assistente virtual.`,
      1000
    );
    this.adicionarChatBox(
      true,
      this.avatarWoman,
      this.nomeAssistenteVirtual,
      "Qual é o seu nome?",
      2000
    );
    this.ultimaPergunta = "nome";
    this.focusTime();
  },
  methods: {
    dateNow() {
      const today = new Date();
      const date =
        today.getFullYear() +
        "-" +
        (today.getMonth() + 1) +
        "-" +
        today.getDate();
      const time =
        today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
      return date + " " + time;
    },
    adicionarChatBox(admin, avatar, nome, msg, time) {
      setTimeout(() => {
        const dataFuncion = this.dateNow();
        this.conversation.unshift({
          admin: admin,
          avatar: avatar,
          nome: nome,
          msg: msg,
          data: dataFuncion,
        });
        this.scroll();
      }, time);
    },
    onEnter() {
      if (this.message) {
        this.send();
      }
    },
    send() {
      this.loading = true;
      const mensagemUsuario = this.message;
      if (this.podeEscrever) {
        this.adicionarChatBox(false, this.avatarBot, "Você", this.message, 0);
      }
      this.podeEscrever = false;
      this.disabled = true;
      this.message = "";
      this.resposta(mensagemUsuario);
      this.loading = false;
    },
    resposta(mensagemUsuario) {
      let primeiraMsgUsuario;
      let proximaMsgUsuario;
      let proximaPergunta;
      const msgLower = mensagemUsuario.toLowerCase();
      switch (true) {
        case this.ultimaPergunta.includes("nome"):
          primeiraMsgUsuario = `${mensagemUsuario}.. que nome lindo!`;
          proximaMsgUsuario = "Qual a sua idade?";
          proximaPergunta = "idade";
          break;
        case this.ultimaPergunta.includes("idade"):
          primeiraMsgUsuario = `${mensagemUsuario}.. está na flor da idade!`;
          proximaMsgUsuario = "Você estuda alguma coisa?";
          proximaPergunta = "estudo";
          break;
        case this.ultimaPergunta.includes("estudo"):
          primeiraMsgUsuario = msgLower.includes("sim")
            ? "Que ótimo, estudar é sempre bom!"
            : "Que pena, estudar é sempre bom!";
          proximaMsgUsuario = "Você trabalha atualmente?";
          proximaPergunta = "trabalho";
          break;
        case this.ultimaPergunta.includes("trabalho"):
          primeiraMsgUsuario = "Entendi!";
          proximaMsgUsuario =
            "Estou finalizando sua sessão, obrigada por conversar comigo! Até breve.";
          proximaPergunta = "adeus";
          this.disabled = true;
          break;
        case this.ultimaPergunta.includes("adeus"):
          primeiraMsgUsuario =
            "Estou finalizando sua sessão, obrigada por conversar comigo! Até breve.";
          proximaPergunta = "";
          this.disabled = true;
          break;
        default:
          primeiraMsgUsuario =
            "Não entendi, poderia repetir novamente com outras palavras para que eu possa lhe ajudar.";
          break;
      }
      this.ultimaPergunta = proximaPergunta;
      this.adicionarChatBox(
        true,
        this.avatarWoman,
        this.nomeAssistenteVirtual,
        primeiraMsgUsuario,
        1000
      );
      if (proximaPergunta && proximaMsgUsuario) {
        this.adicionarChatBox(
          true,
          this.avatarWoman,
          this.nomeAssistenteVirtual,
          proximaMsgUsuario,
          2000
        );
      }
      setTimeout(() => {
        this.disabled = this.ultimaPergunta.includes("adeus") ? true : false;
        this.podeEscrever = true;
        this.focusTime();
      }, 2000);
    },
    focusTime() {
      document.getElementById("inputEnviar").focus();
    },
    scroll() {
      let element = document.getElementById("scroll");
      if (element.scrollTop !== 0) {
        console.log("RESET TO BOTTOM CHAT - NO REMOVE THIS LOG");
        setTimeout(() => {
          element.scrollTop = 0;
        }, 50);
      }
    },
  },
};
</script>

<style>
.container-chat {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  background: white;
  margin: 2% auto;
  max-width: 540px !important;
  height: 80vh;
  border-radius: 5px;
  padding: 2%;
}
.conversation {
  overflow-y: auto;
  display: flex;
  flex-direction: column-reverse;
  padding: 0 2%;
}
.admin {
  display: flex;
  justify-content: left;
}
.usuario {
  display: flex;
  justify-content: right;
}
.usuario span {
  color: #0f3460;
}
.card {
  margin-bottom: 20px;
}
.v-card__title {
  padding-top: 0px;
  padding-bottom: 0px;
}
.msg {
  padding: 0px 15px !important;
  width: 100%;
  text-align: left;
  font-size: 1rem !important;
}
.text-flex > div {
  display: flex;
  flex-flow: wrap-reverse;
}
::-webkit-scrollbar {
  width: 12px;
  position: absolute;
}
::-webkit-scrollbar-track {
  background-color: #d1c51f;
  height: 80px;
}
::-webkit-scrollbar-thumb {
  background: #0f5091;
  border-radius: 2px;
}
::-webkit-scrollbar-thumb:hover {
  background: lighten(#0f5091, 5%);
  transition: 0.5s;
}
</style>
