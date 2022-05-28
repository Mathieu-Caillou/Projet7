<template>
  <div>
    <div class="container-profile">
      <div>
        <div>
          <button class="btn-icons" type="button" title="Modifier votre profil" @click="displayModal">
            <PencilIcon/>
          </button>
          <modify-user v-show="modifyUser" @close="closeModal" />
          <button class="btn-icons" type="button" title="Supprimer votre profil" @click="deleteUser(user.userId)">
            <TrashIcon/>
          </button>
        </div>
      </div>
      <div>
        <div>
          <img v-if="user.picture !== null" :src="user.picture" alt="photo de profil"/>
          <h1> {{ user.username }} </h1>
          <p> {{ user.lastName }} {{ user.firstName }} </p>
          <p> Membre depuis le {{ moment(user.birth).format("[le] DD MMMM YYYY") }}</p>
        </div>
      </div>
      <div>
        <div>
          <MailIcon class="icons"/>
          <p>{{ user.email }}</p>
        </div>
        <div>
          <ChatIcon class="icons"/>
          <p>A propos</p>
        </div>
        <div>
          <p> {{ user.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import moment from "moment";
import modifyUser from "../components/UserModifyModal.vue";
import {
  PencilIcon,
  MailIcon,
  ChatIcon,
  TrashIcon,
} from "@heroicons/vue/solid";

export default {
  name: "Profil",
  components: { modifyUser, PencilIcon, MailIcon, ChatIcon, TrashIcon },

  data: () => ({
    modifyUser: false, // La modale ne doit pas d'afficher initialement
  }),

  beforeMount() {
    this.$store.dispatch("getUserInfos"); // On veut récupérer les infos de l'user dès le chargement
  },
  created: function() {
    this.moment = moment; // Permet le formatage de la date de création
    moment.locale("fr");
  },

  methods: {
    displayModal() {
      this.modifyUser = true;
      this.$router.push("/profil/modify");
    },

    closeModal() {
      this.modifyUser = false;
      this.$router.push("/profil");
    },

    deleteUser(id) {
      this.$store.dispatch("deleteOneUser", id);
      this.$store.dispatch("logOut").then(() => { // Pour éviter des erreurs la session se ferme après la suppression de l'user
        this.$router.push("/"); 
      });
    },
  },

  computed: {
    ...mapState({
      user: (state) => state.user,
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