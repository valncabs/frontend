<script>
  import { onMount } from "svelte";  

  let abierto = true;
  let rol = "";

  function toggleMenu() {
    abierto = !abierto;
  }

  onMount(() => {
    rol = localStorage.getItem("rol");
  });

  function logout() {
    localStorage.removeItem("token");
    localStorage.removeItem("rol");
    window.location.href = "/";
  }

  function selectTab(event) {
    document.querySelectorAll(".nav-link").forEach(link => {
      link.classList.remove("active");
    });

    event.currentTarget.classList.add("active");
  }
</script>

<div class="menu-btn" on:click={toggleMenu}>
<svg xmlns="http://www.w3.org/2000/svg" height="26px" viewBox="0 -960 960 960" width="26px" fill="#ffffff">
<path d="M120-240v-80h720v80H120Zm0-200v-80h720v80H120Zm0-200v-80h720v80H120Z"/>
</svg>
</div>

<aside class:cerrado={!abierto}>

<div class="perfil">
<svg xmlns="http://www.w3.org/2000/svg" height="60px" viewBox="0 -960 960 960" width="60px" fill="#e8eaed">
<path d="M234-276q51-39 114-61.5T480-360q69 0 132 22.5T726-276q35-41 54.5-93T800-480q0-133-93.5-226.5T480-800q-133 0-226.5 93.5T160-480q0 59 19.5 111t54.5 93Zm146.5-204.5Q340-521 340-580t40.5-99.5Q421-720 480-720t99.5 40.5Q620-639 620-580t-40.5 99.5Q539-440 480-440t-99.5-40.5ZM480-80q-83 0-156-31.5T197-197q-54-54-85.5-127T80-480q0-83 31.5-156T197-763q54-54 127-85.5T480-880q83 0 156 31.5T763-763q54 54 85.5 127T880-480q0 83-31.5 156T763-197q-54 54-127 85.5T480-80Z"/>
</svg>

{#if abierto}
<h3>{rol}</h3>
{/if}

</div>

<nav>

<a href="/Tabla_Crud" class="nav-link active" on:click={selectTab}>
<span class="nav-icon">
<svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="M480-560h200v-80H480v80Zm0 240h200v-80H480v80Zm-63.5-223.5Q440-567 440-600t-23.5-56.5Q393-680 360-680t-56.5 23.5Q280-633 280-600t23.5 56.5Q327-520 360-520t56.5-23.5Zm0 240Q440-327 440-360t-23.5-56.5Q393-440 360-440t-56.5 23.5Q280-393 280-360t23.5 56.5Q327-280 360-280t56.5-23.5ZM200-120q-33 0-56.5-23.5T120-200v-560q0-33 23.5-56.5T200-840h560q33 0 56.5 23.5T840-760v560q0 33-23.5 56.5T760-120H200Zm0-80h560v-560H200v560Zm0-560v560-560Z"/></svg>
</span>
{#if abierto}<span>Registro</span>{/if}
</a>

<a href="/Dashboard" class="nav-link">
<span class="nav-icon">
<svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="M280-280h80v-200h-80v200Zm320 0h80v-400h-80v400Zm-160 0h80v-120h-80v120Zm0-200h80v-80h-80v80ZM200-120q-33 0-56.5-23.5T120-200v-560q0-33 23.5-56.5T200-840h560q33 0 56.5 23.5T840-760v560q0 33-23.5 56.5T760-120H200Zm0-80h560v-560H200v560Zm0-560v560-560Z"/></svg>
</span>

{#if abierto}
<span>Dashboard</span>
{/if}

</a>
</nav>

<button class="logout" on:click={logout}>
<span>
    <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e8eaed"><path d="M480-120v-80h280v-560H480v-80h280q33 0 56.5 23.5T840-760v560q0 33-23.5 56.5T760-120H480Zm-80-160-55-58 102-102H120v-80h327L345-622l55-58 200 200-200 200Z"/></svg>
</span>
{#if abierto}<span>Cerrar sesión</span>{/if}
</button>

</aside>

<style>

aside{
position:fixed;
left:0;
top:0;
height:100vh;
width:260px;
background:#128802;
color:white;
display:flex;
flex-direction:column;
padding-top:80px;
transition:width .3s;
}

aside.cerrado{
width:70px;
}

.menu-btn{
position:fixed;
top:15px;
left:15px;
z-index:10;
background:#128802;
padding:8px;
border-radius:8px;
cursor:pointer;
transition:transform .2s;
}

.menu-btn:hover{
transform:scale(1.1);
}

.perfil{
display:flex;
flex-direction:column;
align-items:center;
gap:10px;
margin-bottom:40px;
}

nav{
display:flex;
flex-direction:column;
gap:10px;
}

.nav-link{
display:flex;
align-items:center;
gap:15px;
padding:12px 20px;
color:white;
text-decoration:none;
transition:.2s;
}

.nav-link:hover{
background:rgba(255,255,255,0.1);
transform:translateX(3px);
}

.nav-icon{
width:30px;
display:flex;
justify-content:center;
}

aside.cerrado .nav-link{
justify-content:center;
}

.active{
background:rgba(255,255,255,0.2);
border-left:4px solid white;
}

.logout{
margin-top:auto;
background:#e77e3c;
border:none;
color:white;
padding:12px;
cursor:pointer;
display:flex;
align-items:center;
justify-content:center;
gap:10px;
transition:.3s;
}

.logout:hover{
background:#e77e3c;
transform:scale(1.05);
}

aside.cerrado .logout{
justify-content:center;
}

</style>