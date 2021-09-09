<template>
  <v-app id="inspire">

    <v-navigation-drawer
    v-model="drawer"
    app
    >
      <v-sheet
        class="py-4 d-flex justify-center"
      >
        <!-- class="pa-4" -->
        <v-avatar
        
        size="200"
        >
          <v-img
            :src="$store.state.contato.foto"
            alt="John"
          ></v-img>

        </v-avatar>

      </v-sheet>
      <v-divider></v-divider>
        
        <div
        class="d-flex justify-center font-weight-bold text-h3 text--secondary"
        >
          {{ $store.state.contato.nome }}
        </div>

      <v-divider></v-divider>
    </v-navigation-drawer>



    <v-main
    style="background-color: rgba(0,0,0,.1)"
    >

      <!-- style="margin: 23px 0 0 -13px" -->
      <v-btn
        style="margin: 23px 0 0 -13px; z-index: 3000"
        @click="drawer = !drawer"
        color="primary"
        fab
        dark
        small
        absolute
        top
        left
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>

      <v-container
        class="py-8 px-6"
        fluid
      >
        <v-row
        style="height: 100%"
        >
          <v-col
            cols="12"
          >
            <!-- <v-subheader class="">Últimas mensagens</v-subheader> -->
            
            <v-card
            style="height: 500px"
            >
            <!-- color="indigo lighten-4" -->
              <v-img

              aspect-ratio="1.4"
              max-height="100%"
              src="https://ajcanjusao.cloudimg.io/v7/https://blog.1a23.com/wp-content/uploads/sites/2/2020/02/Desktop.png?w=1024&h=576&org_if_sml=1"
              >

                <v-responsive
                class="overflow-y-auto pt-4 scroll-container"
                spellcheck="0"
                max-height="100%"
                >
                  <v-alert
                  v-for="mensagem, index in mensagens"
                  :key="index"
                  :border="'left'"
                  :color="mensagem.enviada? 'deep-purple accent-4' : 'success'"
                  colored-border
                  elevation="2"
                  class="mx-3"
                  >
                  <!-- width="700" -->
                    <div
                    class="d-flex font-weight-black"
                    >
                      {{mensagem.remetente}}:
                    </div>

                    <div
                    class="d-flex text-h3"
                    >
                      {{ mensagem.conteudo }}
                    </div>

                    <div
                    class="d-flex justify-end font-weight-light"
                    >
                      {{ mensagem.dataDeEnvio.toLocaleDateString()+ ' ' + mensagem.dataDeEnvio.toLocaleTimeString() }}
                    </div>

                  </v-alert>

                </v-responsive>

              </v-img>
            
            </v-card>
            
            <v-divider class="my-3"></v-divider>

            <v-textarea
              solo
              outlined
              name="input-7-4"
              label="Mensagem que será enviada"
              v-model="conteudoMensagem"
              @keyup.prevent.page-up="enviarMsg()"
              autofocus
            ></v-textarea>

          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: true,
      conteudoMensagem:"",
      mensagens:[{
        enviada: false,
        remetente: 'Yuri',
        dataDeEnvio: new Date(),
        conteudo: "Escreva uma mensagem e aperte o botão de enviar."
      }]
    }),
    methods:{
      enviarMsg(){

        if(this.conteudoMensagem){
          let mensagem = {
              enviada: true,
              remetente: 'Você',
              dataDeEnvio: new Date(),
              conteudo: this.conteudoMensagem
          }
  
          this.mensagens.unshift(mensagem);
          this.conteudoMensagem = null;

          let idChat = '1065645850';
          let token = '1919090353:AAGhlahsRIYGtesWLYH-KB0Wzj6p2fZ3MUs';
          const target = "https://api.telegram.org/bot" + token + "/sendMessage?chat_id=" + idChat + "&text=" + mensagem.conteudo;
          
          try{
            fetch(target)
              .then((r)=>r.json()).then((res)=>res)
          }catch(e){
            console.log(e)
          }
        }
      },
      buscaUpdates(){
        fetch("https://api.telegram.org/bot1919090353:AAGhlahsRIYGtesWLYH-KB0Wzj6p2fZ3MUs/getupdates")
          .then((r)=>r.json()).then((res)=>{
            res.result.forEach(hit => {
              
              if(!this.mensagens.filter(msg => msg.conteudo == hit.message.text)[0]){ // Verifica se a mensagem buscada já existe
                
                // se a mensagem não existir coloca ela na tela
                this.mensagens.unshift({
                  enviada: false,
                  remetente: hit.message.from.first_name,
                  dataDeEnvio: new Date(hit.message.date),
                  conteudo: hit.message.text
                })
              }

            });
            // console.log(res)
        });
      },
      sleep(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
      }
    },
    async created(){
      this.buscaUpdates()
      let n = 0;
      do{
        await this.sleep(30000)
        this.buscaUpdates()
        n++
      }while(n < 2000)
      window.location.href = "."
    },
  }
</script>

<style scoped>
#inspire{
  max-width: 100%;
  height: 50%;
}

.texto{
  text-align: end !important;
  min-width: 100% !important;
  border: 1px solid black;
}

.scroll-container{
  overflow-y: scroll;
  height:90vh;
}
::-webkit-scrollbar {
	width: 10px;
	height: 10px;
}
::-webkit-scrollbar-button:start:decrement,
::-webkit-scrollbar-button:end:increment  {
	display: none;
}
::-webkit-scrollbar-track-piece  {
	background-color: #474747;
	-webkit-border-radius: 3px;
}
::-webkit-scrollbar-thumb:vertical {
	background-color: rgb(116, 116, 116);
	-webkit-border-radius: 3px;
}

</style>