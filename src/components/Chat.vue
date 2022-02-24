<template>
    <div class="container-chat">
        <div class="conversation" id="scroll">
            <div
                v-for="(conversa, index) in this.conversation"
                :key="index"
                :id="`chat-${index}`"
                :style="conversa.admin ? 'display:flex; justify-content:left' : 'display:flex; justify-content:right'">
                <v-card
                    class="mx-auto card"
                    :color="conversa.admin ? '#0F3460' : '#FBBB08'"
                    dark
                    width="400"
                >
                    <v-card-title>
                        <v-list-item-avatar color="grey darken-3">
                        <v-img
                            class="elevation-6"
                            alt="Imagem avatar"
                            :src="conversa.avatar"
                        ></v-img>
                        </v-list-item-avatar>
                        <span class="text-h6 font-weight-light">{{conversa.nome}}</span>
                    </v-card-title>

                    <v-card-text class="text-h2 font-weight-bold msg">
                        {{conversa.msg}}
                    </v-card-text>

                    <v-card-actions>
                    <v-list-item>
                        <v-row
                            class="right"
                            justify="end"
                        >
                            <span class="subheading">{{conversa.data}}</span>
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
                    clearable
                    label="Mensagem"
                    v-on:keyup.enter="onEnter"
                    type="text"
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
            message: '',
            loading: false,
            conversation: [],
        }
    },
    mounted() {
        const today = new Date();
        const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
        const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
        const dateTime = date +' '+ time;
        this.conversation.push({
            admin: true,
            avatar: 'https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light',
            nome: 'Barbara Frost',
            msg: 'Olá, no que posso ajudar?',
            data: dateTime,
        });
    },
    methods: {
        onEnter: function() {
            this.send();
        },
        send() {
            this.loading = true;
            const today = new Date();
            const date = today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
            const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
            const dateTime = date +' '+ time;
            this.conversation.push({
                admin: false,
                avatar: 'https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light',
                nome: 'Você',
                msg: this.message,
                data: dateTime,
            });
            this.scroll('scroll');
            this.loading = false;
            this.message = '';
        },
        scroll(id) {
            let element = document.getElementById(id);
            element.scrollTop = element.scrollHeight - element.clientHeight;
        },
    }
}
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
        scroll-behavior: smooth;
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
        transition: .5s;
    }
</style>