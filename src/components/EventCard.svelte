<script lang="ts">
	import { goto } from '$app/navigation';
	//import { formatDate } from '../helpers/formatDate';
	import type { Member } from '../types';

	export let eventDate: string;
	export let eventDescription: string;
	export let hostName: string;
	export let hostPhoto: string;
	export let hostemail: string;
	export let eventName: string;
	export let guestName: string;
	export let guestDesignation: string;
	export let guestPhoto: string;
	export let members: Member[];

</script>

<article
	on:click={()=>goto(`/join/${eventName}`)}
	
	
	class="bg-secondary p-8 rounded-lg hover:bg-blue-800 hover:scale-110 duration-300 transition-all cursor-pointer"
>
	<div class="flex flex-col">
		<h1 class="capitalize text-2xl font-bold tracking-widest">{eventName}</h1>
		<p class="py-2 text-gray-300 font-semibold">{eventDescription}</p>
		<!-- host -->
		<p>Host</p>
		<div class="flex space-x-2 items-center">
			<img src={hostPhoto} alt={hostName} class="h-10 w-10 rounded-full object-cover" />
			<div class="flex flex-col">
				<p>{hostName}</p>
				<p>{hostemail}</p>
			</div>
		</div>
		<!-- guest -->
		<p class="pt-4">Guest</p>
		<div class="flex space-x-2 items-center">
			<img src={guestPhoto} alt={guestName} class="h-10 w-10 rounded-full object-cover" />
			<div class="flex flex-col">
				<p>{guestName}</p>
				<p>{guestDesignation}</p>
			</div>
		</div>
		<p class="text-gray-400 mt-4">Event Date: {eventDate}</p>
		<p>Members -> {members.length}</p>
	</div>
</article>
