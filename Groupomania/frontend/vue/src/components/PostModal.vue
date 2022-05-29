<template>
  <div @click.stop>
    <div>
      <div>
        <div role="dialog" aria-modal="true" aria-label="modal création de post">
          <div>
            <button type="button" aria-label="fermer" @click="close" class="btn-icons">
              <XCircleIcon/>
            </button>
          </div>
            <h2>Créer un post</h2>
            <form>
              <div>
                <textarea v-model="message"
                  class="Post-Text" type="text" placeholder="Votre message" aria-label="Ecrire un message"/>
                <p>La publication doit contenir au moins un message, qui peut être agrémenté par une image issue d'un fichier, ou d'un lien.<br />
                  Les images doivent utiliser les formats suivants : .jpeg, .png, .gif
                </p>
                <input @change="uploadFile" ref="file" name="image" type="file" accept="image/png, image/jpeg, image/gif" aria-label="Rajouter un fichier"  />
                <input v-model="link" type="text" placeholder="Votre lien" aria-label="Rajouter un lien"/>
              </div>
              <div>
                <button type="button" @click=" submitPost(); close();" :disabled="!validatedFields" :class="{ 'opacity-25 cursor-not-allowed': !validatedFields }">
                  <span v-if="messages == 'Post publié !'">Publication en cours...</span>
                  <span v-else>Publier</span>
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
  name: "postModal",

  components: { XCircleIcon },

  data: function() {
    return {
      message: "",
      file: null,
      link: null,
    };
  },

  computed: {
    validatedFields: function() {
      if (this.message != "") {
        return true;
      } else {
        return false;
      }
    },

    ...mapState({ messages: (state) => state.message }),
  },

  methods: {
    uploadFile() {
      const file = this.$refs.file.files[0];
      this.file = file;
    },

    submitPost: function() {
      const formData = new FormData(); // Permet de transmettre le formulaire dans un format adapté au backend

      formData.append("message", this.message);

      if (this.link !== null) {
        formData.append("link", this.link);
      }
      if (this.file !== null) {
        formData.append("image", this.file);
      }
      this.$store.dispatch("createPost", formData);
    },

    resetForm() {
      // Permet de reset le formulaire
      this.message = "";
      this.link = null;
      this.$refs.file.value = null;
    },

    close() {
      this.$emit("close");
      this.resetForm(); // Reset du formulaire à la fermeture de la modal
    },
  },
};
</script>

<style>
.btn-icons {
  width: 50px;
}
.Post-Text {
    width: 35%;
    border: 3px black solid;
    height: 63px;
    box-shadow: 1px 1px 8px 1px black;
}
</style>
