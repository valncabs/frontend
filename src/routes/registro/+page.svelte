<script>
  import { goto } from '$app/navigation';

  let correo = '';
  let nombres = '';
  let apellidos = '';
  let cedula = '';
  let edad = '';
  let username = '';
  let password = '';
  let repeatPassword = '';

  let error = '';
  let loading = false;

  async function handleSubmit(event) {
    event.preventDefault();

    error = '';
    loading = true;

    // =========================
    // VALIDACIONES
    // =========================
    if (
      !correo ||
      !nombres ||
      !apellidos ||
      !cedula ||
      !edad ||
      !username ||
      !password ||
      !repeatPassword
    ) {
      error = 'Completa todos los campos';
      loading = false;
      return;
    }

    if (password !== repeatPassword) {
      error = 'Las contraseñas no coinciden';
      loading = false;
      return;
    }

    try {
      const response = await fetch("http://127.0.0.1:8000/usuarios/",
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            nombre: nombres,
            apellido: apellidos,
            cedula: cedula,
            edad: Number(edad),
            usuario: username,
            correo: correo,
            contrasena: password
          })
        }
      );

      const data = await response.json();

      if (!response.ok) {
        throw new Error(data.detail || 'Error al registrar usuario');
      }

      // Si todo sale bien
      goto('/inicio_sesion');

    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  }
</script>

<div
  class="container-fluid min-vh-100 d-flex justify-content-center align-items-center bg-image"
>
  <div class="card p-4 shadow-lg glass-card">
    <h1 class="text-center mb-4">Regístrate</h1>

    <form on:submit={handleSubmit}>
      <input class="form-control mb-3" type="email" placeholder="Correo" bind:value={correo} required />
      <input class="form-control mb-3" type="text" placeholder="Nombres" bind:value={nombres} required />
      <input class="form-control mb-3" type="text" placeholder="Apellidos" bind:value={apellidos} required />
      <input class="form-control mb-3" type="text" placeholder="Cédula" bind:value={cedula} required />
      <input class="form-control mb-3" type="number" placeholder="Edad" bind:value={edad} required />
      <input class="form-control mb-3" type="text" placeholder="Nombre de usuario" bind:value={username} required />
      <input class="form-control mb-3" type="password" placeholder="Contraseña" bind:value={password} required />
      <input class="form-control mb-3" type="password" placeholder="Confirmar contraseña" bind:value={repeatPassword} required />

      {#if error}
        <div class="alert alert-danger py-2">{error}</div>
      {/if}

      <button class="btn btn-orange w-100 mt-2" type="submit" disabled={loading}>
        {loading ? 'Registrando...' : 'Registrar'}
      </button>
    </form>

    <p class="text-center mt-3">
      ¿Ya tienes una cuenta?
      <a href="/inicio_sesion" class="fw-bold text-orange">Inicia sesión</a>
    </p>
  </div>
</div>

<style>
  .bg-image {
    background-image: url('$lib/assets/fondo_registro.jpeg');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    padding: 20px;
  }

  .glass-card {
    width: 100%;
    max-width: 420px;
    border-radius: 20px;
    background: rgba(255, 255, 255, 0.25);
    backdrop-filter: blur(15px);
    -webkit-backdrop-filter: blur(15px);
  }

  .btn-orange {
    background-color: #ce4003db;
    color: white;
    border-radius: 30px;
  }

  .btn-orange:hover {
    background-color: black;
    color: white;
  }

  .text-orange {
    color: #ce4003db;
  }

  @media (max-width: 576px) {
    .glass-card {
      padding: 20px;
    }
  }
</style>