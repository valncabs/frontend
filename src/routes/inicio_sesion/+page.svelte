<script>
  import { goto } from '$app/navigation';
  import logo from '$lib/assets/logo.png';

  let username = '';
  let password = '';
  let error = '';
  let success = '';
  let loading = false;
  let showPassword = false;

  function togglePassword() {
    showPassword = !showPassword;
  }

  async function handleSubmit(event) {
    event.preventDefault();

    error = '';
    success = '';
    loading = true;

    if (!username.trim() || !password.trim()) {
      error = 'Completa todos los campos';
      loading = false;
      return;
    }

    try {
      const response = await fetch('https://fastapi-findpaw-1.onrender.com/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          usuario: username.trim(),
          contrasena: password.trim()
        })
      });

      const data = await response.json();
      if (!response.ok) {
        throw new Error(data.detail || 'Error en el inicio de sesión');
      }

      if (data.access_token) {
        localStorage.setItem('token', data.access_token);
        localStorage.setItem('rol', data.rol);
      }

      success = 'Inicio de sesión exitoso';

  if (data.rol && data.rol.toLowerCase() === "administrador") {
  goto("/admin");
} else {
  goto("/usuario");
}
      username = '';
      password = '';

    } catch (err) {
      error = err.message || 'Error al iniciar sesión';
    } finally {
      loading = false;
    }
  }
</script>

<head>
  <title>FindPaw - Inicio de Sesión</title>
</head>
<div class="container-fluid min-vh-100 p-0">
  <div class="row g-0 min-vh-100">

    <!-- PANEL IZQUIERDO -->
    <div class="col-12 col-lg-6 inicio-sesion-body d-flex flex-column align-items-center justify-content-center">

      <h1>Inicio Sesión</h1>

      {#if error}
        <p id="error-message">{error}</p>
      {/if}

      {#if success}
        <p id="success-message">{success}</p>
      {/if}

      <form on:submit={handleSubmit}>

        <!-- USUARIO -->
        <div>
          <label>
           <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="M367-527q-47-47-47-113t47-113q47-47 113-47t113 47q47 47 47 113t-47 113q-47 47-113 47t-113-47ZM160-160v-112q0-34 17.5-62.5T224-378q62-31 126-46.5T480-440q66 0 130 15.5T736-378q29 15 46.5 43.5T800-272v112H160Zm80-80h480v-32q0-11-5.5-20T700-306q-54-27-109-40.5T480-360q-56 0-111 13.5T260-306q-9 5-14.5 14t-5.5 20v32Zm296.5-343.5Q560-607 560-640t-23.5-56.5Q513-720 480-720t-56.5 23.5Q400-673 400-640t23.5 56.5Q447-560 480-560t56.5-23.5ZM480-640Zm0 400Z"/></svg>
          </label>

          <input
            type="text"
            placeholder="Nombre de usuario"
            bind:value={username}
            required
          />
        </div>

        <!-- CONTRASEÑA -->
        <div class="password-container">
          <label>
          <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="M240-80q-33 0-56.5-23.5T160-160v-400q0-33 23.5-56.5T240-640h40v-80q0-83 58.5-141.5T480-920q83 0 141.5 58.5T680-720v80h40q33 0 56.5 23.5T800-560v400q0 33-23.5 56.5T720-80H240Zm0-80h480v-400H240v400Zm296.5-143.5Q560-327 560-360t-23.5-56.5Q513-440 480-440t-56.5 23.5Q400-393 400-360t23.5 56.5Q447-280 480-280t56.5-23.5ZM360-640h240v-80q0-50-35-85t-85-35q-50 0-85 35t-35 85v80ZM240-160v-400 400Z"/></svg>
          </label>

          <input
            type={showPassword ? "text" : "password"}
            placeholder="Contraseña"
            bind:value={password}
            required
          />

          <button type="button" class="toggle-password" on:click={togglePassword}>

            {#if showPassword}
              <!-- OJO ABIERTO -->
            <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="M607.5-372.5Q660-425 660-500t-52.5-127.5Q555-680 480-680t-127.5 52.5Q300-575 300-500t52.5 127.5Q405-320 480-320t127.5-52.5Zm-204-51Q372-455 372-500t31.5-76.5Q435-608 480-608t76.5 31.5Q588-545 588-500t-31.5 76.5Q525-392 480-392t-76.5-31.5ZM214-281.5Q94-363 40-500q54-137 174-218.5T480-800q146 0 266 81.5T920-500q-54 137-174 218.5T480-200q-146 0-266-81.5ZM480-500Zm207.5 160.5Q782-399 832-500q-50-101-144.5-160.5T480-720q-113 0-207.5 59.5T128-500q50 101 144.5 160.5T480-280q113 0 207.5-59.5Z"/></svg>
            {:else}
              <!-- OJO CERRADO -->
              <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="m644-428-58-58q9-47-27-88t-93-32l-58-58q17-8 34.5-12t37.5-4q75 0 127.5 52.5T660-500q0 20-4 37.5T644-428Zm128 126-58-56q38-29 67.5-63.5T832-500q-50-101-143.5-160.5T480-720q-29 0-57 4t-55 12l-62-62q41-17 84-25.5t90-8.5q151 0 269 83.5T920-500q-23 59-60.5 109.5T772-302Zm20 246L624-222q-35 11-70.5 16.5T480-200q-151 0-269-83.5T40-500q21-53 53-98.5t73-81.5L56-792l56-56 736 736-56 56ZM222-624q-29 26-53 57t-41 67q50 101 143.5 160.5T480-280q20 0 39-2.5t39-5.5l-36-38q-11 3-21 4.5t-21 1.5q-75 0-127.5-52.5T300-500q0-11 1.5-21t4.5-21l-84-82Zm319 93Zm-151 75Z"/></svg>
            {/if}

          </button>
        </div>

        <button type="submit" disabled={loading}>
          {loading ? 'Entrando...' : 'Iniciar Sesión'}
        </button>

        <p class="register-text">
          ¿No tienes cuenta? <a href="/registro">Regístrate aquí</a>
        </p>

      </form>
    </div>

    <!-- PANEL DERECHO -->
    <div class="col-lg-6 d-none d-lg-flex align-items-center justify-content-center bg-light">
      <img src={logo} alt="Logo" class="img-fluid" />
    </div>

  </div>
</div>

<style>

@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;900&display=swap');

:global(body){
  margin:0;
}

.password-container {
  position: relative;
  display: flex;
}

.password-container input {
  padding-right: 50px; /* espacio real para el ojo */
}
.password-container:focus-within .toggle-password svg {
  fill: #107D02;
}
.toggle-password {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-80%);
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}
.inicio-sesion-body{
  background-color:#107D02;
  padding:40px 20px;
  color:white;
  font-family:Poppins,sans-serif;
}

h1{
  font-size:3rem;
  font-weight:900;
  text-transform:uppercase;
}

form{
  width:min(400px,100%);
  margin-top:20px;
  display:flex;
  flex-direction:column;
  gap:10px;
}

form>div{
  display:flex;
}

form label{
  height:50px;
  width:50px;
  background-color:#1bce03db;
  display:flex;
  justify-content:center;
  align-items:center;
  border-radius:10px 0 0 10px;
}

form input{
  flex:1;
  height:50px;
  padding:1em;
  border-radius:0 10px 10px 0;
  border:2px solid rgba(255,255,255,0.2);
  border-left:none;
  background-color:rgba(255,255,255,0.2);
  color:white;
}

form input:focus{
  outline:none;
  border-color:white;
}

form div:focus-within label {
  background-color: white;
}

form div:focus-within label svg {
  fill: #107D02;
}

form div:focus-within input {
  background-color: white;
  color: #107D02;
  border-color: white;
}

form button{
  margin-top:15px;
  border:none;
  border-radius:1000px;
  padding:.85em;
  background-color:#1bce03db;
  color:#107D02;
  font-weight:600;
  cursor:pointer;
  transition:.3s;
}

form button:hover{
  background-color:white;
}

#error-message{
  color:#ff6b6b;
}

#success-message{
  color:#86f679;
}

.register-text{
  text-align:center;
}

.register-text a{
  color:#86f679;
  font-weight:600;
}

</style>