<template>
  <nav style="display: flex;">
      <div class="container-icon">
          <router-link v-if="status == 'isConnected'" to="/wall">
        <img
          class="icon-home"
          src="../assets/icons/icon-white-navbar.png"
          alt="Groupomania logo"
        />
      </router-link>
      <router-link v-else to="/">
        <img
          class="icon-home"
          src="../assets/icons/icon-white-navbar.png"
          alt="Groupomania logo"
        />
      </router-link>
      </div>
      <div class="align-center">   
          <template v-if="status == 'isConnected'">
        <router-link to="/wall" class="mlmr font-semibold-white">Acceuil</router-link>
        <router-link to="/profil" class="mlmr font-semibold-white">Profil</router-link>
        <router-link to="/login" @click="logOut()" class="">Déconnexion</router-link>
      </template>

      <template v-else>
        <router-link to="/login" class="mlmr font-semibold-white">Connexion</router-link>
        <router-link to="/signup" class="mlmr font-semibold-white">Inscription</router-link>
      </template>
      </div>
  </nav>
</template>

<script>
import { mapState } from "vuex"; // Nous avons besoin d'une valeur du store Vuex pour gérer afficher des élements du header

export default {
  name: "Header",

  data() {
    return {
      view: {
        atTopOfPage: true,
      },
    };
  },

  beforeMount() {
    window.addEventListener("scroll", this.handleScroll); // On écoute l'évènement scroll
  },

  computed: {
    ...mapState(["status"]), // Le statut "isConnected" sera recherché dans le store

     userConnected () { // Permet de faire pointer le router vers l'id de l'utilisateur courant
      return this.$store.state.user.userId 
    }
  },

  methods: {
    handleScroll() {
      // Cette méthode s'active lors du scroll
      if (window.pageYOffset > 0) {
        if (this.view.atTopOfPage) this.view.atTopOfPage = false;
      } else {
        if (!this.view.atTopOfPage) this.view.atTopOfPage = true;
      }
    },

    logOut: function() {
      // Retour sur la page de connexion après la déconnexion
      this.$store.dispatch("logOut").then(() => {
        this.$router.push("/login");
      });
    },
  },
};
</script>

<style>
nav {
     display: flex;
     background: black;
     justify-content: space-between;
     padding: 20px;
 }
 .icon-home {
     height: 2.5rem;
 }
  a {
    text-decoration: none;
 }
.mlmr {
    margin-left: 0.75rem;
    margin-right: 0.75rem;
    text-decoration: none !important;
}
.mlmr:hover {
  color: white;
}
.font-semibold-white {
    color: white;
    font-weight: 600;
}

.align-center {
  align-items: center;
}
</style>