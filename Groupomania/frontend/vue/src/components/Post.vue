<template>
  <div>
    <div class="Post-container">
      <div>
        <div class="container-picture">
          <img class="profile-picture" v-if="post.User.picture !== null" :src="post.User.picture" alt="photo de profil"/>
        </div>
        <div class="Post-top">
          <router-link :to="`/profil/${post.User.id}`">
            <!-- Lien vers le profil du créateur du post -->
            <p class="Post-username">{{ post.User.username }}</p>
          </router-link>
          <p> {{ moment(post.createdAt).format("[le] DD MMMM YYYY") }} </p>
        </div>
        <!-- Si l'user est le propriétaire du post ou l'admin on autorise la modification ou la suppression -->
        <div class="Btn-container" v-if=" $store.state.user.userId == post.User.id || $store.state.user.isAdmin == true">
          <button type="button" title="Modifer ce post" @click="modifyPost(post.id)" class="btn-icons Modif-Btn">
          <PencilIcon/>
          </button>
          <modifyModal v-show="showModal" @close="closeModifyPost"/>
          <button type="button" title="Supprimer ce post" @click="deletePost(post.id)" class="btn-icons">
            <TrashIcon/>
          </button>
        </div>
      </div>

      <div v-if="post.imageURL !== null">
        <img  :src="post.imageURL" class="media-container" alt="image postée"/>
      </div>
      <div v-if="post.link !== null">
        <img  :src="post.link" class="media-container" alt="gif posté"/>
      </div>
      <div>
        <p class="post-txt">{{ post.message }}</p>
      </div>

      <!-- Affichage du bouton like et commentaires, et de leurs compteurs -->
      <div class="flex">
        <div>
          <button type="button" title="Liker" @click="likePost(post.id)" class="btn-icons Modif-Btn">
            <HeartIcon :class="postLiked"/>
          </button>
          <p>{{ post.Likes.length }}</p>
        </div>
        <div>
          <button type="button" title="Accéder aux commentaires" @click="showComments = !showComments" class="btn-icons">
            <AnnotationIcon/>
          </button>
          <p> {{ post.Comments.length }}</p>
        </div>
      </div>
      <!-- Section commentaires -->
      <div v-show="showComments">
        <form>
          <div>
            <textarea v-model="data.comment" type="text" rows="1" placeholder="Votre commentaire" aria-label="Ecrire un commentaire"/>
            <button type="button" title="Envoyer le commentaire" @click="commentPost(post.id)" class="btn-icons">
              <ArrowCircleRightIcon :disabled="!validatedFields" :class="{ 'opacity-25 cursor-not-allowed': !validatedFields }"/>
            </button>
          </div>
        </form>
        <div>
          <div v-for="comment of post.Comments" :key="comment.id" >
            <img v-if="comment.User.picture !== null" :src="comment.User.picture" alt="photo de profil"/>
            <div>
              <div>
                <p class="blue-txt"> {{ comment.User.username }} </p>
                <p class="date-txt"> {{ moment(comment.createdAt).format("[le] DD MMMM YYYY [à] HH:mm") }} </p>
                <p>{{ comment.comment }}</p>
                <div v-if=" $store.state.user.userId == comment.UserId || $store.state.user.isAdmin == true">
                  <button type="button" title="Supprimer ce commentaire" @click="deleteComment(comment.id)">
                  <TrashIcon class="btn-icons"/>
                  </button>
                </div>
              </div>
              
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import modifyModal from "../components/PostModifyModal.vue";

import {
  PencilIcon,
  TrashIcon,
  StatusOnlineIcon,
  HeartIcon,
  AnnotationIcon,
  ArrowCircleRightIcon,
} from "@heroicons/vue/solid";

export default {
  name: "Posts",

  components: {
    modifyModal,
    PencilIcon,
    TrashIcon,
    StatusOnlineIcon,
    HeartIcon,
    AnnotationIcon,
    ArrowCircleRightIcon,
  },

  data: () => ({
    comment: "",

    showComments: false, // La div des commentaires ne s'affiche pas à la base

    showModal: false, // Ni la modale permettant de modifier un post

    data: {
      comment: "",
    },
  }),

  props: { // On récupére post du parent, donc de Wall.vue
    post: {
      type: Object,
    },
  },

  created: function() {
    this.moment = moment; // Permet le formatage de la date du post
    moment.locale("fr");
  },

  methods: {
    modifyPost(id) {
      (this.showModal = true), this.$router.push(`/posts/${id}`);
      this.$store.dispatch("getPostByID", id); // Récupération du contenu du post dès ouverture de la modale
    },

    closeModifyPost() {
      this.showModal = false;
      this.$router.push("/wall");
    },

    deletePost(id) {
      this.$store.dispatch("deleteOnePost", id); 
      location.reload();
      location.reload();
    },

    likePost(id) {
      this.$store.dispatch("postLike", id);
      location.reload();
      location.reload();
    },

    commentPost(id) {
      this.$store.dispatch("addComment", {
        id: id,
        data: this.data,
      });
      this.data.comment = ""; // Supprime le commentaire du data après l'envoi, permet de fermer la div
    },

    deleteComment(id) {
      this.$store.dispatch("deleteOneComment", id);
      location.reload();
      location.reload();
    },
  },

  computed: {
    postLiked() {
      // Le bouton like reste en rose si l'user a déjà liké l'article
      const userId = this.$store.state.user.userId;
      let userLike = this.post.Likes.map((id) => id.UserId.toString()); // A convertir pour comparaison avec userId
      //console.log(typeof userId);
      //console.log(typeof userLike);
      if (userLike.includes(userId)) {
        return "";
      } else {
        return "";
      }
    },

    validatedFields: function() {
      if (this.data.comment != "") {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style>
.btn-icons {
  width: 50px;
}
.Post-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: grey;
    margin-top: 20px;
}
.Post-top {
  display: flex;
  margin-right: 15px;
}
.Post-username {
  margin-right: 15px;
}
.Btn-container { 
  text-align: center;
}
.Modif-Btn {
  margin-right: 15px;
}
.media-container{
  margin: 15px 0 15px 0;
  max-width: 200px;
}
.post-txt {
  color: white;
}
.flex {
  display: flex;
}
.blue-txt {
  color: blue;
}
.date-txt {
  font-style: italic;
}
.profile-picture {
  border: 3px black solid;
  max-height: 90px;
}
.container-picture {
  text-align: center;
}

</style>
