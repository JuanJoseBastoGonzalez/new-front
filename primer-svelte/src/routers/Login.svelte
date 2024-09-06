<script lang="ts">
    import { Link, navigate } from  'svelte-routing';
    import { showDashboar } from '../stores/store';
    import { axiosTools } from "../stores/store";
    import { onMount } from 'svelte';
    let user:String="";
    let password:string="";
    let token:any="";
   async function toggDashboard(){
if ( user =="" && password ==""){
    alert("Ingresar usuario y contraseña")
}else if (user =="")
{
    alert("ingresar usuario")
}else if (password ==""){
    alert("inresar contraseña")
}else{
    alert("ok")
   try {
                const response = await fetch("http://localhost:8091/auth/login", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({ user, password }),
                });
                if (!response.ok) {
                    const errorData = await response.json();
                    console.error("Error:", errorData.message);
                    return;
                }
                const data = await response.json();
                if (data.token) {
                    token = data.token;
                    localStorage.setItem("token", token); 
                    console.log("JWT Token:", token);
                    navigate("/Dashboard"); 
                } else {
                    console.error("Token no encontrado en la respuesta");
                }
            } catch (error) {
                console.error("Error en la solicitud:", error);
            }
    }
}
onMount(() => {
        token = localStorage.getItem("token") || "";
    });


    
</script>

<div>
    <input bind:value={user} placeholder="usuario" required>
    <input bind:value={password} placeholder="Contraseña" required>
   <p on:click={toggDashboard}>Entrar</p> 


</div>

