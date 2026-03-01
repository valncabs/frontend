<script>
  import logo from '$lib/assets/logo.png';

  let username = '';
  let password = '';
  let error = '';
  let success = '';
  let loading = false;

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
      const response = await fetch('http://127.0.0.1:8000/auth/login', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          usuario: username.trim(),
          contrasena: password.trim()
        })
      });

      const data = await response.json();

      console.log("Status:", response.status);
      console.log("Data:", data);

      if (!response.ok) {
        throw new Error(data.detail || 'Error en login');
      }

      //  LOGIN EXITOSO
      console.log("LOGIN EXITOSO");
      console.log("Token recibido:", data.access_token);

      // Guardar token
      localStorage.setItem('token', data.access_token);

      success = 'Inicio de sesión exitoso';

      username = '';
      password = '';

    } catch (err) {
      console.log(" ERROR:", err.message);
      error = err.message || 'Error al iniciar sesión';
    } finally {
      loading = false;
    }
  }
</script>


<div class="container-fluid min-vh-100 p-0">
  <div class="row g-0 min-vh-100">


    <div class="col-12 col-lg-6 inicio-sesion-body d-flex flex-column align-items-center justify-content-center">
      <h1>Inicio Sesión</h1>

      {#if error}
        <p id="error-message">{error}</p>
      {/if}

      <form on:submit={handleSubmit}>

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

        <div>
          <label>
           <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="M240-80q-33 0-56.5-23.5T160-160v-400q0-33 23.5-56.5T240-640h40v-80q0-83 58.5-141.5T480-920q83 0 141.5 58.5T680-720v80h40q33 0 56.5 23.5T800-560v400q0 33-23.5 56.5T720-80H240Zm0-80h480v-400H240v400Zm296.5-143.5Q560-327 560-360t-23.5-56.5Q513-440 480-440t-56.5 23.5Q400-393 400-360t23.5 56.5Q447-280 480-280t56.5-23.5ZM360-640h240v-80q0-50-35-85t-85-35q-50 0-85 35t-35 85v80ZM240-160v-400 400Z"/></svg>
          </label>

          <input
            type="password"
            placeholder="Contraseña"
            bind:value={password}
            required
          />
        </div>

        <button type="submit" disabled={loading}>
          {loading ? 'Entrando...' : 'Iniciar Sesión'}
        </button>

        <p class="register-text">
          ¿No tienes cuenta? <a href="/registro">Regístrate aquí</a>
        </p>

      </form>
    </div>


    <div class="col-lg-6 d-none d-lg-flex align-items-center justify-content-center bg-light">
      <img src={logo} alt="Logo" class="img-fluid logo-img" />
    </div>

  </div>
</div>

<style>

@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;900&display=swap');

:global(body) {
  margin: 0;
}


/* PANEL VERDE */
.inicio-sesion-body {
  background-color: #107D02;
  width: 45%;
  min-width: 420px; /* mínimo razonable */
  padding: 40px 20px;
  border-radius: 0 20px 20px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: white;
  font-family: Poppins, sans-serif;
}

/* RESPONSIVE */
@media (max-width: 992px) {



  .inicio-sesion-body {
    width: 100%;
    min-width: 100%;
    border-radius: 0;
  }
}

/* TITULO */
h1 {
  font-size: 3rem;
  font-weight: 900;
  text-transform: uppercase;
}

/* FORM */
form {
  width: min(400px, 100%);
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

form > div {
  display: flex;
}

/* ICONOS (NO TOCADOS) */
form label {
  height: 50px;
  width: 50px;
  background-color: #1bce03db;
  color: #107D02;
  border-radius: 10px 0 0 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}

form input {
  flex: 1;
  height: 50px;
  padding: 1em;
  border-radius: 0 10px 10px 0;
  border: 2px solid rgba(255,255,255,0.2);
  border-left: none;
  background-color: rgba(255,255,255,0.2);
  color: white;
}

form input:focus {
  outline: none;
  border-color: white;
  background-color: rgba(255,255,255,0.3);
}

form div:focus-within label {
  background-color: white;
  color: #107D02;
}

::placeholder {
  color: #ffffff;
}

form button {
  margin-top: 15px;
  border: none;
  border-radius: 1000px;
  padding: .85em;
  background-color: #1bce03db;
  color: #107D02;
  font-weight: 600;
  cursor: pointer;
  transition: 0.3s;
}

form button:hover {
  background-color: white;
  transform: translateY(-5px);
}

#error-message {
  color: #ff6b6b;
}

.register-text {
  width: 100%;
  text-align: center;
}

.register-text a {
  color: #86f679;
  font-weight: 600;
}

.register-text a:hover {
  color: #083f01;
}
</style>