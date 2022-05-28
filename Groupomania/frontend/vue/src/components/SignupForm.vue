<template>
  <div class="signup-container">
      <form>
          <div class="input-container">
            <input v-model="firstName" class="input" type="text" placeholder="Votre nom" aria-label="nom">
            <input v-model="lastName" class="input" type="text" placeholder="Votre prénom" aria-label="prénom">
            <input v-model="username" class="input" type="text" placeholder="Votre nom d'utilisateur" aria-label="nom d'utilisateur">
            <input v-model="email" class="input" type="text" placeholder="Votre email" aria-label="email">
            <input v-model="password" class="input" type="password" placeholder="Votre mot de passe" aria-label="mot de passe">
          </div>
          <div>
              <button type="button" aria-label="inscription" @click="createAccount();" class="signup-btn" :disabled="!validatedFields" :class="{ 'opacity-25 cursor-not-allowed': !validatedFields }">
               <span v-if="status == 'loading'">Création du compte en cours...</span>
               <span v-else>Créer mon compte</span>
              </button>
          </div>
          <div>
              <p v-if="status == 'error'" class="text-error">Inscription impossible, veuillez vérifier les informations fournies</p>
              
          </div>
      </form>
      <div>
          <p class="desc-text">
            Le mot de passe doit contenir au minimum 8 caractères, au moins une
            majuscule, une minuscule, et un chiffre. Il ne doit pas contenir
            d'espaces.
          </p>
      </div>
  </div>
</template>

<script>
import { mapState } from "vuex"; // mapState nous permet de récupérer des propriétés de state du store

export default {
      name: "signupForm",

  data: function() {
    return {
      firstName: "",
      lastName: "",
      username: "",
      email: "",
      password: "",
    };
  },

  computed: {
    // Fonction qui va permettre de désactiver le bouton d'inscription si les champs ne sont pas remplis
    validatedFields: function() {
      if (
        this.firstName != "" &&
        this.lastName != "" &&
        this.username != "" &&
        this.email != "" &&
        this.password != ""
      ) {
        return true;
      } else {
        return false;
      }
    },
    ...mapState(["status"]), // On souhaite récupérer les stats status du store
  },

  methods: {
    createAccount: function() {
      const self = this; // Afin d'utiliser this dans un autre contexte
      this.$store
        .dispatch("createAccount", {
          // On propage l'action createAccount du store avec l'objet présent
          firstName: this.firstName,
          lastName: this.lastName,
          username: this.username,
          email: this.email,
          password: this.password,
        })
        .then(
          function() {
            self.$router.push('/login'); // Puis on retourne sur la page login afin de se connecter
          },
          function(error) {
            console.log(error);
          }
        );
    },
  },
}
</script>

<style>
.text-error {
    color: red;
    text-align: center;
}
.signup-title {
  text-align: center;
  padding: 2.5rem;
}
.input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.input {
  border-radius: 6px;
  border: 3px solid black;
  margin: 21px 0 0 0;
  width: 45%;
}
.signup-container { 
  background-color: gray;
  width: 50%;
  margin: 0;
  padding: 0;
  display: block;
  margin: 0 auto 0 auto;
}
.desc-text { 
  color: white;
  text-align: center;
  font-style: italic;
}
.signup-btn {
  display: block;
  padding-top: 0.5rem;
  padding-bottom: 0.5rem;
  padding-left: 1rem;
  padding-right: 1rem;
  border-radius: 0.375rem;
  border-width: 2px;
  margin: 20px auto 10px auto;
} 
</style>