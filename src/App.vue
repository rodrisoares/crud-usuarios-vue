<template>
  <div id="app" class="container bg-light">
    <h1 class="pt-5">CRUD - Cadastro de usuários</h1>
    <hr class="bg-dark" />
    
    <h2 >Cadastrar usuários:</h2>
    <b-button @click="exibirCadastro" v-if="exibirBtnCadastro" class="w-30 mt-2" variant="primary"
        >Cadastrar
    </b-button>
    
      <div v-if="exibirTelaCadastro">
      
        <!--Campo de nome:-->
        <b-card>
            <label  for="input-live">Nome:</label>
              <b-form-input
                class="w-50 p-1" 
                id="input-live" 
                placeholder="Digite seu nome"
                v-model="usuario.nome" 
                :state="isNameStateEmpty"
                aria-describedby="input-live-help input-live-feedback"
                type="text"
                size="lg"
                trim
                >
              </b-form-input>
          
              <!-- Campo de erro quando o nome for menor que 3 caracteres -->
              <b-form-invalid-feedback id="input-live-feedback" v-if="msgNomeInvalido">
                  Digite no mínimo 3 caracteres!
              </b-form-invalid-feedback>
        </b-card>

        <!--Campo de email:-->
        <b-card>
            <label  for="input-live">Email:</label>
              <b-form-input 
                class="w-50 p-1"
                id="input-live" 
                placeholder="Digite seu e-mail"
                v-model="usuario.email" 
                :state="isEmailStateEmpty"
                aria-describedby="input-live-help input-live-feedback"
                type="text"
                size="lg"
                trim
                >
              </b-form-input>
          
              <!-- Campo de erro quando o email for inválido -->
              <b-form-invalid-feedback id="inputLiveFeedback" v-if="msgEmailInvalido">
                  Digite um email válido!
              </b-form-invalid-feedback>
        </b-card>

        <!--Campo de cargo:-->
        <b-card>
            <label  for="input-live">Cargo:</label>
              <b-form-input
                class="w-50 p-1" 
                id="input-live" 
                placeholder="Digite seu cargo"
                v-model="usuario.cargo" 
                :state="isCargoStateEmpty"
                aria-describedby="input-live-help input-live-feedback"
                type="text"
                size="lg"
                trim
                >
              </b-form-input>
          
              <!-- Campo de erro quando o cargo for menor que 3 caracteres -->
              <b-form-invalid-feedback id="input-live-feedback" v-if="msgCargoInvalido">
                  Digite no mínimo 3 caracteres!
              </b-form-invalid-feedback>
        </b-card>

        <!--Campo de login:-->
        <b-card>
            <label  for="input-live">Login:</label>
              <b-form-input 
                class="w-50 p-1"
                id="input-live" 
                placeholder="Digite seu login"
                v-model="usuario.login" 
                :state="isLoginStateEmpty"
                type="text"
                size="lg"
                trim
                >
              </b-form-input>

              <!-- Campo de erro quando o login for inválido -->
              <b-form-invalid-feedback id="inputLiveFeedback" v-if="msgLoginInvalido">
                Digite no mínimo 3 caracteres!
              </b-form-invalid-feedback>
        </b-card>

        <!--Campo de senha:-->
        <b-card> 
          <label  for="input-live">Senha:</label>
              <b-form-input
                class="w-50 p-1"
                id="input-live" 
                v-model="usuario.senha"
                :type="showPassword ? 'text' : 'password'"
                placeholder="Digite sua senha"
                :state="isPasswordStateEmpty"
                size="lg"
              >
              </b-form-input>
                <img class="eyePassword"
                  @click="showPassword = !showPassword" 
                  src="./assets/eye.png" 
                  alt="imagem exibir/ocultar senha"
                >
                <b-form-invalid-feedback id="inputLiveFeedback" v-if="msgSenhaInvalida">
                  A senha deve ter pelo menos 8 caracteres!
                </b-form-invalid-feedback>
        </b-card>

        <!--Campo de confirmar senha:-->
        <b-card> 
          <label  for="input-live">Confirmar senha:</label>
            <b-form-input
              class="w-50 p-1"
              :type="showPasswordConfirm ? 'text' : 'password'"
              v-model="usuario.confirmsenha"
              placeholder="Confirme sua senha"
              :state="isConfirmPasswordState"
              >
            </b-form-input>
                <img class="eyePasswordConfirm" 
                  @click="showPasswordConfirm = !showPasswordConfirm" 
                  src="./assets/eye.png" 
                  alt="imagem exibir/ocultar senha"
                >
                <b-form-invalid-feedback v-if="msgSenhaConfirmInvalida">
                  As senhas são diferentes!
                </b-form-invalid-feedback>
        </b-card>
      
        <b-card>
          <b-button @click="salvarDados" class="w-30 mt-2" variant="primary"
            >Salvar
          </b-button>
          <b-button @click="cancelarDados"  class="w-30 mt-2 ml-2 "  variant="dark"
            >Cancelar
          </b-button>
        </b-card>
    </div>
      
      <hr class="bg-dark" />
      <h2 v-if="usuarios">Usuários:</h2>
      
      <b-button @click="esconderUsuarios" v-if="esconderBtnUsuarios" class="w-30 mt-2" variant="dark"
          >Esconder usuários
      </b-button>
      <b-button @click="exibirUsuarios" v-if="exibirBtnUsuarios" class="w-30 mt-2" variant="dark"
          >Exibir usuários
      </b-button>
      
      <div v-if="exibirTelaUsuarios">

        <table class="table table-striped table-bordered">
        
                <thead>
                    <tr>
                        <th>Nome</th>
                        <th>Email</th>
                        <th>Login</th>
                        <th>Cargo</th>
                        <th>Ação</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(usuario, id) in usuarios" :key="id">
                        <td>{{usuario.nome }} </td>
                        <td>{{usuario.email}}</td>
                        <td>{{usuario.login}}</td>
                        <td>{{usuario.cargo}}</td>
                        <b-button 
                          @click="carregarForm(id)"  
                          class="w-30 mt-2 ml-2" 
                          variant="warning"
                          >Editar</b-button
                        >

                        <b-button
                          @click="deletarUsuario(id)"
                          class="w-30 mt-2 ml-2 " 
                          variant="danger"
                          >Deletar</b-button
                        >
                    </tr>
                </tbody>
        </table>
        <h3 v-show="this.usuarios.length < 1 " >Não há usuários para exibir!</h3>
      </div>
  </div>
</template>

<script>
export default {
  computed: {
      isNameStateEmpty() {
        if (this.usuario.nome) {
          return this.isValidName(this.usuario.nome);
        }
        return null;
      },
      isEmailStateEmpty() {
        if (this.usuario.email) {
          return this.isValidEmail(this.usuario.email);
        }
        return null;
      },
      isLoginStateEmpty() {
        if (this.usuario.login) {
          return this.isValidLogin(this.usuario.login);
        }
        return null;
      },
      isCargoStateEmpty() {
        if (this.usuario.cargo) {
          return this.isValidCargo(this.usuario.cargo);
        }
        return null;
      },
      isPasswordStateEmpty() {
        if (this.usuario.senha) {
          return this.isValidPassword(this.usuario.senha);
        }
        return null;
      },
      isConfirmPasswordState() {
        if (this.usuario.confirmsenha) {
          return this.isValidPasswordConfirm(this.usuario.confirmsenha);
        }
        return null;
      },

    },
  data: () => ({
    usuarios: [],
    id: null,
    usuario: {
      nome: "",
      email: "", 
      login:"",
      cargo:"",
      senha:"",
      confirmsenha: "",
    },
    msgNomeInvalido: false,
    msgEmailInvalido: false,
    msgLoginInvalido: false,
    msgCargoInvalido: false,
    msgSenhaInvalida: false,
    msgSenhaConfirmInvalida: false,
    showPassword: false,
    showPasswordConfirm: false,
    exibirTelaCadastro: false,
    exibirBtnCadastro: true,
    exibirTelaUsuarios: true,
    exibirBtnUsuarios: false,
    esconderBtnUsuarios: true,
  }),
  created() {
    this.obterUsuarios();
  },
  methods: {
    exibirCadastro() {
      this.exibirTelaCadastro = true
      this.exibirBtnCadastro = false
    }, 
    esconderUsuarios() {
      this.exibirTelaUsuarios = false
      this.exibirBtnUsuarios = true
      this.esconderBtnUsuarios = false
      
    }, 
    exibirUsuarios() {
      this.exibirTelaUsuarios = true
      this.exibirBtnUsuarios = false
      this.esconderBtnUsuarios = true
    }, 
    isValidName() {
      this.msgNomeInvalido = true;
      return this.usuario.nome.length >= 3 ? true : false; 
    },
    isValidEmail() {
      this.msgEmailInvalido = true;
      return /^[^@]+@\w+(\.\w+)+\w$/.test(this.usuario.email) ? true : false;
    },
    isValidLogin() {
      this.msgLoginInvalido = true;
      return this.usuario.login.length >= 3 ? true : false; 
    },
    isValidCargo() {
      this.msgCargoInvalido = true;
      return this.usuario.cargo.length >= 3 ? true : false; 
    },
    isValidPassword() {
      this.msgSenhaInvalida = true;
      return this.usuario.senha.length >= 8 ? true : false; 
    },
    isValidPasswordConfirm() {
      this.msgSenhaConfirmInvalida = true;
      return this.usuario.senha == this.usuario.confirmsenha &&
         this.usuario.confirmsenha != "" 
         ? true 
         : false
    },
    limparForm() {
      this.usuario = {
        nome: "",
        email: "",
        cargo:"",
        login:"",
        senha:"",
        confirmsenha: "",
      };
      this.id = null;
    },
  
    salvarDados() {

        if(this.isNameStateEmpty && this.isEmailStateEmpty && this.isPasswordStateEmpty &&this.isConfirmPasswordState) {
      
          const metodo = this.id ? "patch" : "post";
          const finalUrl = this.id ? `/${this.id}.json` : ".json";

          this.$httpClient[metodo](`/usuarios${finalUrl}`, this.usuario)
            .then(() => {
              this.limparForm();
              this.exibirTelaCadastro= false
              this.exibirBtnCadastro= true

              this.$swal({
                icon: "success",
                title: "Sucesso",
                text:
                  metodo == "patch"
                    ? "O usuário foi alterado com sucesso!"
                    : "O usuário foi criado com sucesso!",
              });

              this.obterUsuarios();
            })
            .catch(() => {
              this.$swal({
                icon: "error",
                title: "Ops...",
                text: "Algum problema ocorreu durante a sua requisição :(",
              });
            });
        }

      else {
        this.$swal({
            icon: "error",
            title: "Ops...",
            text: "Erro ao salvar usuario :(",
        });
      }
      
    },
    cancelarDados() {
      this.limparForm()
      this.exibirTelaCadastro = false
      this.exibirBtnCadastro = true
    },
    obterUsuarios() {
      this.$httpClient.get("usuarios.json").then((resp) => {
        this.usuarios = resp.data;
      });
    },

    carregarForm(userID) {
      this.exibirTelaCadastro= true
      this.exibirBtnCadastro = false
      this.id = userID;
      this.usuario = { ...this.usuarios[userID] };
    },

    deletarUsuario(userID) {
      this.$swal({
        icon: "warning",
        title: "Tem certeza?",
        text:
          "Uma vez deletado, você não terá como recuperar os dados deste usuário!",
        buttons: {
          cancel: "Cancelar",
          ok: {
            text: "Sim, tenho certeza",
            value: true,
          },
        },
        dangerMode: true,
      }).then((value) => {
        if (value) {
          this.$httpClient.delete(`/usuarios/${userID}.json`).then(() => {
            this.limparForm();
            this.obterUsuarios();
          });

          this.$swal({
            icon: "success",
            title: "Okay",
            text: "O usuário foi excluído com sucesso!",
          });
        } else {
          this.$swal("O usuário não foi excluído :)");
        }
      });
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  font-size: 1.5rem;
}

#app h1 {
  text-align: center;
  margin: 50px;
}

img {
  width: 30px;
  height: 30px; 
  
}
.eyePassword{
  margin-left: 50%;
  top: 70px;
  position: absolute;
}
.eyePasswordConfirm{
  margin-left: 50%;
  top: 70px;
  position: absolute;
}
</style>
