<template>
  <div>
    <div class="container-profile">
      <div v-if=" $store.state.user.userId == users.id || $store.state.user.isAdmin == true">
        <div>
          <button class="btn-icons" type="button" title="Supprimer ce profil" @click="deleteUser(users.id)">
            <TrashIcon/>
          </button>
        </div>
      </div>
      <div>
        <div>
          <img v-if="users.picture !== null" :src="users.picture" alt="photo de profil"/>
          <h1> {{ users.username }} </h1>
          <p> {{ users.lastName }} {{ users.firstName }} </p>
          <p> Membre depuis le {{ moment(users.createdAt).format("[le] DD MMMM YYYY") }}</p>
        </div>
      </div>
      <div>
        <div>
          <MailIcon class="icons"/>
          <p>{{ users.email }}</p>
        </div>
        <div>
          <ChatIcon class="icons"/>
          <p>A propos</p>
        </div>
        <div>
          <p> {{ users.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import moment from "moment";

import { MailIcon, ChatIcon, TrashIcon } from "@heroicons/vue/solid";

export default {
  name: "Profil",
  components: { MailIcon, ChatIcon, TrashIcon },

  data: () => ({
    showModal: false, // La modale ne doit pas d'afficher initialement
  }),

  beforeMount() {
    this.$store.dispatch("getOtherUser", this.$route.params.id); // On récupére les infos de l'user en fonction de l'id de l'URL
  },

  created: function() {
    this.moment = moment; // Permet le formatage de la date de création
    moment.locale("fr");
  },

  methods: {
    deleteUser(id) {
      this.$store.dispatch("deleteOneUser", id);
      this.$router.push("/wall");
    },
  },

  computed: {
    ...mapState({
      users: (state) => state.users, // On map les infos user récupérées avec le dispatch
    }),
  },
};
</script>

<style>
.icons {
  max-width: 50px;
}
.container-profile {
  text-align: center;
}
.btn-icons {
  width: 50px;
}
</style>