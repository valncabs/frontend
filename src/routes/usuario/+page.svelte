<script> 
    import { onMount } from "svelte";
    import Sidebar from "../../components/Sidebar.svelte";
  let rol = "";

  onMount(() => {
    rol = localStorage.getItem("rol");

    if (rol !== "usuario") {
      window.location.href = "/";
    }
  });
</script>

