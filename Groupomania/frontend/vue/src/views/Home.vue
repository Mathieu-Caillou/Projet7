<template>
  <div class="card-display">
      <h1>Bienvenue à vous !</h1>
      <div class="card">
        <img class="card-img-top" src="../assets/icons/icon-above-font.png" alt="Card image cap">
        <div class="card-body">
            <h5 class="card-title">Groupomania est fière de vous présenter son site</h5>
            <p class="card-text">Que diriez vous de vous inscrire pour en savoir plus ? <br/> Alors n'hésitez pas et rejoignez nous ! 
            </p>
        </div>
       </div>
  </div>
</template>

<script>
export default {
    name: "Home",
    methods: {
    // Utilisation d'instances router pour créer des ancres
    toLogin() {
      this.$router.push({ path: "/login" });
    },
    toSignup() {
      this.$router.push({ path: "/signup" });
    },
  },
}
</script>

<style>
.align-center {
  align-items: center;
}
.card-display {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.card {
    margin-top: 20px;
    width: 100%;
    align-items: center;
    border: none;
}
.card-img-top {
    width: 298px;
}
.card-text {
    text-align: center;
}
.card-title {
  text-align: center;
}
</style>