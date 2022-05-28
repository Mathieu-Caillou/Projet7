<template>
  <div>
    <div>
      <div class="container">
        <img class="pp" v-if="user.picture !== null" :src="user.picture" alt="photo de profil"/>
        <button type="button" @click="displayModal">
          <div>
            <h2>
              Bonjour {{ user.username }}. Que partagez-vous ce {{ dayName }} ?
            </h2>
          </div>
          <post-modal v-show="showModal" @close="closeModal"/>
        </button>
      </div>

      <div v-if="$store.state.posts == 0" class="text-publication">
        <p>Personne n'a encore publié par ici !</p>
      </div>

      <postView v-for="post of posts" :key="post.id" :post="post" :id="post.id">
        <!-- Le contenu itéré sera affiché via le composant post.vue -->
      </postView>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";

import postModal from "../components/PostModal.vue";
import postView from "../components/Post.vue";

export default {
  name: "Wall",
  components: { postModal, postView },

  data: () => ({
    showModal: false, // La modale ne doit pas d'afficher initialement
  }),

  beforeMount() {
    // On demande les informations de l'user ainsi que les posts disponibles avant le rendu
    this.$store.dispatch("getUserInfos");
    this.$store.dispatch("getAllPosts");
  },

  methods: {
    displayModal() {
      this.showModal = true;
      this.$router.push("/posts/add");
    },

    closeModal() {
      this.showModal = false;
      this.$router.push("/wall");
    },
  },

  computed: {
    ...mapState({
      user: (state) => state.user,
      posts: (state) => state.posts,
    }),

    dayName() {
      // Récupération du jour actuel
      const dateObj = new Date();
      const weekday = dateObj.toLocaleString("fr-FR", { weekday: "long" });
      return `${weekday}`;
    },
  },
};
</script>

<style>
.container {
    text-align: center;
    margin-top: 50px;
    padding-bottom: 50px;
}

.text-publication {
  text-align: center;
}
.pp {
  max-height: 132px;
}
</style>