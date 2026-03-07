<script>
import Sidebar from "../../components/Sidebar.svelte";
import { onMount } from "svelte";
import jsPDF from "jspdf";
import autoTable from "jspdf-autotable";

const API_URL = "https://fastapi-findpaw-1.onrender.com";

let mascotas = [];

let editando = false;
let mascotaEditando = null;

/* FORMULARIO */

let nuevaMascota = {
usuario_id: 13,
nombre:"",
raza:"",
color:"",
tamano:"",
edad_aprox:"",
descripcion:"",
ubicacion:"",
recompensa:0
};

/* CARGAR */

async function cargarMascotas(){

try{

const res = await fetch(`${API_URL}/get_mascotas`);

mascotas = await res.json();

}catch(error){
console.error(error);
}

}

onMount(()=>{
cargarMascotas();
});

/* CREAR */

async function crearMascota(){

try{

const res = await fetch(`${API_URL}/create_mascota`,{
method:"POST",
headers:{
"Content-Type":"application/json"
},
body: JSON.stringify(nuevaMascota)
});

const data = await res.json();

console.log(data);

if(!res.ok){
alert("Error creando mascota");
return;
}

alert("Mascota creada");

limpiarFormulario();
cargarMascotas();

}catch(error){
console.error(error);
}

}

/* EDITAR */

function editarMascota(m){

editando = true;

mascotaEditando = m.id;

nuevaMascota = {
usuario_id:m.usuario_id,
nombre:m.nombre,
raza:m.raza,
color:m.color,
tamano:m.tamano,
edad_aprox:m.edad_aprox,
descripcion:m.descripcion,
ubicacion:m.ubicacion,
recompensa:m.recompensa
};

}

/* ACTUALIZAR */

async function actualizarMascota(){

try{

const res = await fetch(`${API_URL}/update_mascota/${mascotaEditando}`,{
method:"PUT",
headers:{
"Content-Type":"application/json"
},
body: JSON.stringify(nuevaMascota)
});

if(!res.ok){
alert("Error actualizando");
return;
}

alert("Mascota actualizada");

editando=false;
mascotaEditando=null;

limpiarFormulario();
cargarMascotas();

}catch(error){
console.error(error);
}

}

/* ELIMINAR */

async function eliminarMascota(id){

if(!confirm("¿Eliminar mascota?")) return;

try{

const res = await fetch(`${API_URL}/delete_mascota/${id}`,{
method:"DELETE"
});

if(!res.ok){
alert("Error eliminando");
return;
}

alert("Mascota eliminada");

cargarMascotas();

}catch(error){
console.error(error);
}

}

/* LIMPIAR */

function limpiarFormulario(){

nuevaMascota = {
usuario_id:1,
nombre:"",
raza:"",
color:"",
tamano:"",
edad_aprox:"",
descripcion:"",
ubicacion:"",
recompensa:0
};

editando=false;
mascotaEditando=null;

}

/* PDF */

function exportarPDF(){

const doc = new jsPDF();

doc.text("Mascotas Perdidas",14,15);

autoTable(doc,{
startY:20,
head:[["ID","Nombre","Raza","Color","Tamaño","Edad","Descripcion","Ubicación","Recompensa"]],
body:mascotas.map(m=>[
m.id,
m.nombre,
m.raza,
m.color,
m.tamano,
m.edad_aprox,
m.descripcion,
m.ubicacion,
m.recompensa
])
});

doc.save("mascotas.pdf");

}

</script>

<Sidebar/>

<div class="contenido">

<h2 class="titulo">Mascotas Perdidas</h2>

<div class="botones">

<button class="btn crear" on:click={crearMascota} disabled={editando}>
Crear Mascota
</button>

<button class="btn actualizar" on:click={actualizarMascota} disabled={!editando}>
Actualizar
</button>

<button class="btn exportar" on:click={exportarPDF}>
Exportar PDF
</button>

</div>

<table class="tabla">

<thead>

<tr>

<th>ID</th>
<th>Nombre</th>
<th>Raza</th>
<th>Color</th>
<th>Tamaño</th>
<th>Edad</th>
<th>Descripción</th>
<th>Ubicación</th>
<th>Recompensa</th>
<th>Acciones</th>

</tr>

</thead>

<tbody>

{#each mascotas as m}

<tr>

<td>{m.id}</td>
<td>{m.nombre}</td>
<td>{m.raza}</td>
<td>{m.color}</td>
<td>{m.tamano}</td>
<td>{m.edad_aprox}</td>
<td>{m.descripcion}</td>
<td>{m.ubicacion}</td>
<td>{m.recompensa}</td>

<td>

<button class="btn editar" on:click={()=>editarMascota(m)}>
Editar
</button>

<button class="btn eliminar" on:click={()=>eliminarMascota(m.id)}>
Eliminar
</button>

</td>

</tr>

{/each}

</tbody>

</table>

<h3>{editando ? "Editar Mascota" : "Nueva Mascota"}</h3>

<div class="formulario">

<input placeholder="Nombre" bind:value={nuevaMascota.nombre}>
<input placeholder="Raza" bind:value={nuevaMascota.raza}>
<input placeholder="Color" bind:value={nuevaMascota.color}>
<input placeholder="Tamaño" bind:value={nuevaMascota.tamano}>
<input placeholder="Edad" bind:value={nuevaMascota.edad_aprox}>
<input placeholder="Descripcion" bind:value={nuevaMascota.descripcion}>
<input placeholder="Ubicacion" bind:value={nuevaMascota.ubicacion}>
<input type="number" placeholder="Recompensa" bind:value={nuevaMascota.recompensa}>

</div>

</div>

<style>

.contenido{
margin-left:260px;
padding:30px;
min-height:100vh;
}

.botones{
display:flex;
gap:10px;
margin-bottom:20px;
}

.btn{
border:none;
padding:10px 16px;
border-radius:8px;
cursor:pointer;
}

.crear{
background:#16a34a;
color:white;
}

.actualizar{
background:#f59e0b;
color:white;
}

.exportar{
background:#2563eb;
color:white;
}

.editar{
background:#6366f1;
color:white;
padding:6px 12px;
}

.eliminar{
background:#ef4444;
color:white;
padding:6px 12px;
}

.tabla{
width:100%;
border-collapse:collapse;
}

.tabla th{
background:#111827;
color:white;
padding:10px;
}

.tabla td{
padding:10px;
border-bottom:1px solid #eee;
}

.formulario{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
gap:10px;
margin-top:15px;
}

.formulario input{
padding:10px;
border:1px solid #ccc;
border-radius:6px;
}

</style>