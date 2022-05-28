<template>
    <div @click.stop>
      <div>
        <div class="container-modii-profile">
          <div role="dialog" aria-modal="true" aria-label="modal modification de post">
            <div>
              <button class="btn-icons" type="button" aria-label="fermer" @click="close">
                <XCircleIcon/>
              </button>
            </div>
            <h2>Modifier un profil</h2>
            <form>
              <div>
                <textarea v-model="username" type="text" placeholder="Nom d'utilisateur" aria-label="Changer nom d'utilisateur"/>
              </div>
              <div>
                <textarea v-model="description" type="text" placeholder="A propos de vous" aria-label="Changer de description"/>
              </div>
              <span>Modifier votre photo de profil</span>
              <input @change="uploadFile" ref="file" labelfor="image" name="image" type="file" accept="image/png, image/jpeg, image/gif" aria-label="Changer la photo de profil"/>
              <div class="flex justify-center">
                <button type="button" @click=" updateProfil(); close();" :disabled="!validatedFields" :class="{ 'opacity-25 cursor-not-allowed': !validatedFields }">
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
      username: "",
      email: "",
      description: "",
      file: null,
    };
  },

  computed: {
    ...mapState({
      messages: (state) => state.message,
      user: (state) => state.user,
    }),

    validatedFields: function() {
      if (
        this.username !== "" ||
        this.description !== "" ||
        this.file !== null
      ) {
        return true;
      } else {
        return false;
      }
    },
  },

  methods: {
    uploadFile() {
      const file = this.$refs.file.files[0];
      this.file = file;
    },

    updateProfil: function() {
      // Même schéma que pour la création des posts, mis à part que le payload comporte l'id du post en plus du data

      let id = this.$store.state.user.userId;

      const formData = new FormData();

      if (this.username !== "") {
        formData.append("username", this.username);
      }

      if (this.description !== "") {
        formData.append("description", this.description);
      }
      if (this.file !== null) {
        formData.append("image", this.file);
      }

      this.$store.dispatch("updateUserInfos", {
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
.container-modii-profile {
    margin-bottom: 30px;
}
</style>
