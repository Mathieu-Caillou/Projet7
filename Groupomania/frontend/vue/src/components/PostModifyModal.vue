<template>
    <div @click.stop>
      <div>
        <div>
          <div role="dialog" aria-modal="true" aria-label="modal modification de post">
            <div>
              <button type="button" aria-label="fermer" @click="close" class="btn-icons">
                <XCircleIcon aria-label="fermer"/>
              </button>
            </div>
            <h2>Modifier un post</h2>
            <form>
              <div class="Modif-container">
                <textarea style="width: 80%;" v-model="post.message" class="Post-Text" type="text" placeholder="Votre message" aria-label="Ecrire un message"/>
                <input @change="uploadFile" ref="file" label for="image" name="image" class="" type="file" accept="image/png, image/jpeg, image/gif" aria-label="Rajouter un fichier"/>
                <input v-model="post.link" type="text" placeholder="Votre lien" aria-label="Rajouter un lien"/>
              </div>
              <div>
                <button type="button" @click=" modifyPost(); close();" :disabled="!validatedFields" :class="{ 'opacity-25 cursor-not-allowed': !validatedFields }">
                  <span v-if="messages == 'Post modifié !'">Modification en cours...</span>
                  <span v-else>Modifier</span>
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

</template>

<script>
import { XCircleIcon } from "@heroicons/vue/solid";
import { mapState } from "vuex";

export default {
  name: "modifyModal",

  components: { XCircleIcon },

  data: function() {
    return {
      file: null,
    };
  },

  computed: {
    validatedFields: function() {
      if (
        this.post.message != "" ||
        this.file != null ||
        this.post.link != null
      ) {
        return true;
      } else {
        return false;
      }
    },

    ...mapState({
      messages: (state) => state.message,
      post: (state) => state.post,
    }),
  },

  methods: {
    uploadFile() {
      const file = this.$refs.file.files[0];
      this.file = file;
    },

    modifyPost: function() {
      // Même schéma que pour la création des posts, mis à part que le payload comporte l'id du post en plus du data

      let id = this.$route.params.id;

      const formData = new FormData();

      if (this.post.message !== null) {
        formData.append("message", this.post.message);
      }

      if (this.post.link !== null) {
        formData.append("link", this.post.link);
      }
      if (this.file !== null) {
        formData.append("image", this.file);
      }

      this.$store.dispatch("updatePost", {
        id: id,
        data: formData,
      });
    },

    close() {
      this.$emit("close");
    },
  },
};
</script>

<style>
.btn-icons {
  width: 50px;
}
.Post-Text {
  width: 80%;
  border: 3px black solid;
  height: 63px;
  box-shadow: 1px 1px 8px 1px black;
}
.Modif-container {
  display: flex;
    flex-direction: column;
    align-items: center;
}
</style>
