<script>
import ArrendatarioService from "@/AutoYa/services/arrendatario.service";
import PropietarioService from "@/AutoYa/services/propietario.service";
export default {
  data(){
    return {
      drawer: false,
      items: [
        { label: "Inicio", to: "/init-propie" },
        { label: "Registro", to: "/car-registration-owner" },
        { label: "Notificaciones", to: "/notifications" },
        { label: "Alquiler", to: "/rent-owner" },
      ],
      nombres: '',
      apellidos: '',
      celular: '',
      fechaNacimiento: '',
      antecedentesPenalesPdf: '',
      fotoPerfil: '',
    };
  },
  methods: {
    async actualizarDatosTenant() {
      const arrendatarioId = parseInt(localStorage.getItem("arrendatarioId"));

      if (!arrendatarioId) {
        // Manejar el caso en que no haya un arrendatarioId en el localStorage
        return;
      }

      try {
        const response2 = await ArrendatarioService.getAll();
        // Filtrar la información del propietario por el id almacenado en localStorage
        const arrendatario = response2.data.find(
            (arrendatario) =>
                arrendatario.id === arrendatarioId
        );

        const response = await ArrendatarioService.update(arrendatarioId, {
          nombres: this.nombres,
          apellidos: this.apellidos,
          fechaNacimiento: this.fechaNacimiento,
          telefono: this.celular,
          correo: arrendatario.correo,
          antecedentesPenalesPdf: this.antecedentesPenalesPdf,
          contrasenia: arrendatario.contrasenia,
        });

        localStorage.setItem("fotoTenant", this.fotoPerfil);

        // Puedes manejar la respuesta según tus necesidades
        console.log("Respuesta del servicio de arrendatario:", response);
        this.$toast.add({ severity: 'success', summary: 'Éxito', detail: 'Información actualizada exitosamente.' });
        this.$router.push('/profile-tenant');
      } catch (error) {
        // Puedes manejar el error según tus necesidades
        console.error("Error al actualizar datos del arrendatario:", error);
      }
    },
  }
};
</script>

<template>
  <pv-toast />
  <header>
    <pv-toolbar class="custom-bg custom-toolbar">
      <template #start>
        <img
            src="https://i.postimg.cc/vmZh3LGv/logotransparent-26-06.png"
            alt="Logo"
            style="height: 40px; margin-right: 20px;"
        />
      </template>
      <template #end>
        <div class="flex-column">
          <router-link
              v-for="item in items"
              :to="item.to"
              custom
              v-slot="{ navigate, href }"
              :key="item.label"
          >
            <pv-button
                class="custom-button"
                :href="href"
                @click="navigate"
            >
              {{ item.label }}
            </pv-button>
          </router-link>
          <router-link to="/profile-tenant">
            <img
                src="https://i.postimg.cc/Fs9Z3g3V/usuario-1.png"
                alt="Usuario"
                style="height: 30px; margin-left: 20px; cursor: pointer;"
            />
          </router-link>
        </div>
      </template>
    </pv-toolbar>
  </header>
  <body class="center-container" style="margin-top: 150px;">
  <h1 style="color: #FF7A00;">Actualice sus datos</h1>
  <h2>Nombres</h2><br>
  <pv-input placeholder="Nombres" v-model="nombres" style="font-family: 'Poppins',sans-serif"></pv-input><br>
  <h2>Apellidos</h2><br>
  <pv-input placeholder="Apellidos" v-model="apellidos" style="font-family: 'Poppins',sans-serif"></pv-input><br>
  <h2>Celular</h2><br>
  <pv-input placeholder="Celular" v-model="celular" style="font-family: 'Poppins',sans-serif"></pv-input><br>
  <h2>Fecha de nacimiento</h2><br>
  <pv-input placeholder="Fecha de nacimiento" v-model="fechaNacimiento" style="font-family: 'Poppins',sans-serif"></pv-input><br>
  <h2>URL antecedentes penales</h2><br>
  <pv-input placeholder="Antecedentes penales" v-model="antecedentesPenalesPdf" style="font-family: 'Poppins',sans-serif"></pv-input><br>
  <h2>URL foto de perfil</h2><br>
  <pv-input placeholder="Foto de perfil" v-model="fotoPerfil" style="font-family: 'Poppins',sans-serif"></pv-input><br>
  <Button label="Actualizar datos" class="custom-button2" @click="actualizarDatosTenant">Actualizar</Button>
  </body>
</template>

<style scoped>
/* No va a cambiar*/
body{
  font-family: 'Poppins', sans-serif;
  color: black;
  background-color: white;
}
.custom-bg {
  background-color: white;
}
.custom-button, .font-button {
  background-color: white;
  color: #14131B;
}
.custom-button:hover,
.custom-button:focus {
  background-color: #FF7A00 !important;
  color: white !important;
}

.custom-toolbar {
  border-bottom: 2px solid #ddd;
}
.profile-button {
  text-align: center;
  margin-top: 10px;
}

.buttons {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.font-button {
  margin: 2px 0;
  background-color: black !important;
  color: white !important;
}

.font-button:hover,
.font-button:focus{
  background-color: #14131B !important;
  color: white !important;
}
/*Cosas a cambiar*/
.profile-container {
  display: flex;
  align-items: flex-start;
  justify-content: space-between; /**/
}

.left-column {
  flex: auto;
  display: flex;
  flex-direction: column;
  padding: 20px;
}

.right-column {
  flex: 10;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.title h1{
  margin-top: 0;
}

.size-photo{
  max-width: 50%;
  max-height: 50%;
}

.profile-image-container{
  background-color: #f5f5f5;
  border: 2px solid #ccc;
  padding: 20px;
  border-radius: 10px;
  width: 50%;
  height: 50%;
}

.profile-info{
  margin-top: 40px;
  margin-bottom: 20px;
}

.profile-info p{
  margin: 10px;
  line-height: 2.5;
}

.profile-image{
  display: flex;
  align-items: center;
  justify-content: center;
}

h2, p {
  display: inline;
  margin: 0;
}

.center-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh; /* 100% de la altura de la ventana */
  margin: 0;
}
</style>