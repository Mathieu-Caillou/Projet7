<template>
    <div class="">
        <h1 class="signup-title">Inscription à votre espace Groupomania</h1>
    <div class="">
    <signupForm />
    </div>
    </div>
</template>

<script>

// Utilisation du component formulaire de connexion
import signupForm from '../components/SignupForm.vue';

export default {
    name: 'Signup',
    components: {signupForm}
}
</script>