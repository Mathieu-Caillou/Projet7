<template>
  <div class="login-container">
      <form>
          <div class="input-container">
            <input v-model="username" class="input" type="text" placeholder="Votre nom d'utilisateur" aria-label="nom d'utilisateur">
            <input v-model="password" class="input" type="text" placeholder="Votre mot de passe" aria-label="mot de passe">
          </div>
          <div>
             <button type="button" class="login-btn" aria-label="Connexion" @click="logToAccount();" :disabled="!validatedFields" :class="{ 'opacity-25 cursor-not-allowed': !validatedFields }">
             <span v-if="status =='loading'">Connexion en cours</span>
             <span v-else>Connexion</span>
             </button>
          </div>
          <div>
              <p v-if="status == 'error'" class="text-error">Nom d'utilisateur ou mot de passe incorrect.</p>
              
          </div>
      </form>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
    name: "loginForm",

    data: function() {
        return {
            username: "",
            password: "",
        }
    },
    computed: {
        // Fonction qui va permettre de désactiver le bouton connexion si les champs ne sont pas remplis
        validatedFields: function() {
            if (this.username != "" && this.password != "") {
                return true;
            } else {
                return false;
            }
        },
        ...mapState(["status"]),
    },
    methods: {
        logToAccount: function() {
            const self = this;
            this.$store
            .dispatch("logToAccount", {
                username: this.username,
                password: this.password,
            })
            .then(
                function() {
                    self.$router.push("/wall"); //Après s'être connecté on nous renvoie à la page wall
                },
                function(error) {
                    console.log(error);
                }
            );
        },
    },
};
</script>

<style>
.login-container { 
  background-color: gray;
  width: 50%;
  margin: 0;
  padding: 0;
  display: block;
  margin: 0 auto 0 auto;
}
.input {
  border-radius: 6px;
  border: 3px solid black;
  margin: 21px 0 0 0;
  width: 45%;
}
.text-error {
    color: red;
    text-align: center;
}
.login-btn {
  display: block;
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
  padding-left: 1rem;
  padding-right: 1rem;
  border-radius: 0.375rem;
  border-width: 2px;
  margin: 20px auto 10px auto;
} 
.input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.login-title {
  text-align: center;
  padding: 2.5rem;
}
</style>