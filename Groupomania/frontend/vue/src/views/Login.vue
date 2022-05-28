<template>
    <div>
    <h1 class="login-title">Connexion à votre espace Groupomania</h1>
    <div>
    <loginForm />
    </div>
    </div>
</template>

<script>

// Utilisation du component formulaire de connexion
import loginForm from '../components/LoginForm.vue';

export default {
    name: 'Login',
    components: {loginForm}
}
</script>
