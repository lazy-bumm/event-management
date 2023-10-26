<script lang='ts'>
	import type { User } from "firebase/auth";
	import { authHandlers, authStore } from "../store/store";


  let currentuser:User|null
  authStore.subscribe((value)=>{
    currentuser=value.user
  })
</script>


<header class="bg-secondary p-4 flex justify-between items-center text-white">
	<!-- o -->
	<h1 class=" font-bold text-sm md:text-2xl tracking-widest">R. Event Manager</h1>
	<!-- links -->
	<div class="flex items-center space-x-3 font-semibold ">
		<a href="/main" class="hover:bg-red-800 hover:scale-110  ">Main</a>
		<a href="/eventlist" class="hover:bg-red-800 hover:scale-110"   >Event List</a>
		<a href="/create" class="hover:bg-red-800 hover:scale-110">Create Event</a>
	</div>

{#if currentuser}
    

    <div class="items-center space-x-2 hidden xl:inline-flex">
        <img
            src={currentuser?.photoURL}
            alt={currentuser?.displayName}
            class="h-12 w-12 rounded-full"
        />
        <div class="flex flex-col">
            <p>
                Logged in as : <span class="text-purple-500 italic font-bold">
                   {currentuser?.displayName}
                </span>
            </p>
            <p>{currentuser?.email}</p>
        </div>
        <button on:click={authHandlers.logout} class="bg-white p-2 rounded-full text-black hover:bg-blue-800 hover:scale-110 duration-300 transition-all"
            >Log Out</button
        >
    </div>
    {/if}
    </header>