<template>
  <div class="container">
    <div class="login-card">
      <h1>CineReseñas</h1>
      <p>Tu portal de críticas cinematográficas</p>
      <form id="loginForm" @submit.prevent="iniciarSesion">
        <div class="input-group">
          <label for="username">Usuario</label>
          <input type="text" id="username" v-model="email" required />
        </div>
        <div class="input-group">
          <label for="password">Contraseña</label>
          <input type="password" id="password" v-model="password" required />
        </div>
        <button type="submit" id="loginButton">Iniciar sesión</button>
      </form>

      <p class="register-link">
        ¿No tienes una cuenta? <a href="#">Regístrate</a>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoginPrin",
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    iniciarSesion() {
      if (!this.email || !this.password) {
        this.$q.notify({
          message: "Por favor, completa todos los campos",
          color: "negative",
          icon: "error",
        });
        return;
      }

      const endpointURL = "/api/v1/user/signin";
      const user = {
        email: this.email,
        password: this.password,
      };

      this.$api
        .post(endpointURL, user)
        .then((response) => {
          localStorage.setItem("userData", JSON.stringify(response.data));
          console.log(response);

          this.$q.notify({
            message: "Bienvenido",
            color: "positive",
            icon: "check_circle",
          });

          this.$router.push("/dashboard/movies");
        })
        .catch((error) => {
          this.$q.notify({
            message: "Error al iniciar sesión",
            color: "negative",
            icon: "error",
          });
          console.error(error);
        });
    },
  },
};
</script>

<style>
body,
html {
  height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
  font-family: Arial, sans-serif;
}

.container {
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url("https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.login-card {
  background-color: rgba(0, 0, 0, 0.8);
  padding: 2rem;
  border-radius: 10px;
  color: white;
  width: 100%;
  max-width: 400px;
}

h1 {
  color: #ffd700;
  text-align: center;
  margin-bottom: 0.5rem;
}

p {
  text-align: center;
  color: #ccc;
  margin-bottom: 1.5rem;
}

.input-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  color: #ccc;
}

input {
  width: 100%;
  padding: 0.5rem;
  border: none;
  border-radius: 4px;
  background-color: rgba(255, 255, 255, 0.1);
  color: white;
}

button {
  width: 100%;
  padding: 0.75rem;
  border: none;
  border-radius: 4px;
  background-color: #ffd700;
  color: black;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #ffc000;
}

.register-link {
  margin-top: 1rem;
  font-size: 0.9rem;
}

.register-link a {
  color: #ffd700;
  text-decoration: none;
}

.register-link a:hover {
  text-decoration: underline;
}
</style>
