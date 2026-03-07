<script>
    import Sidebar from "../../components/Sidebar.svelte";
    import { onMount } from "svelte";

  let rol = "";

  onMount(() => {
    rol = localStorage.getItem("rol");

    if (rol !== "Administrador") {
      window.location.href = "/";
    }
  });
</script>


<Sidebar></Sidebar>


