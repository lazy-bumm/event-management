<script lang="ts">
import { doc, setDoc } from 'firebase/firestore';
	import { db, storage } from '$lib/firebase';
	import type { User } from 'firebase/auth';
	import { authStore } from '../../store/store';
	import { ref, uploadBytes, getDownloadURL} from 'firebase/storage';
	import { goto } from '$app/navigation';

let eventName: string;
	let eventDescription: string;
	let eventDate: string;
	let guestName: string;
	let guestPhoto: File | undefined;
	let guestDesignation: string;
	let loading = false;
	let currentUser: User | null = null;
     
	authStore.subscribe((value)=>{
		currentUser=value.user;
	})
	function handleFileInputChange(event: Event) {
		const inputElement = event.target as HTMLInputElement;
		if (inputElement.files && inputElement.files.length > 0) {
			guestPhoto = inputElement.files[0];
		}
	}
   
   async function createEvent(){
	if(eventName==undefined || eventDescription==undefined || loading==true)
	  return alert("Some Fields are empty")
	loading=true;
	const eventInfo = {
			eventName: eventName,
			eventDescription: eventDescription,
			eventDate: eventDate,
			guestName: guestName,
			guestPhoto: await uploadGuestPhoto(),
			guestDesignation: guestDesignation,
			hostName: currentUser?.displayName,
			hostPhoto: currentUser?.photoURL,
			hostemail: currentUser?.email,
			members: []
		};

		try{
			const eventRef=doc(db,'events',eventName)
			setDoc(eventRef,eventInfo,{merge:true})
			goto("/eventlist")
		}
		catch(error){
			console.log(`error when creating event, ${error}`)
		}

		loading=false;
   }

   async function uploadGuestPhoto(){
	if(!guestPhoto)return null;
	try{
		const storageRef=ref(storage,"guest_photos/"+guestPhoto.name)
		await uploadBytes(storageRef,guestPhoto)
		const downloadURL=await getDownloadURL(storageRef);
		return downloadURL;
	} catch(error){

	}
   }
</script>

<main class="text-gray-100 mt-10">
	<div>
		<!-- Input box -->
		<div class="max-w-4xl mx-auto bg-secondary rounded-lg flex flex-col p-5">
			<h1 class="text-center text-white text-2xl">Create</h1>

			<div class="flex flex-col my-4">
				<label for="event-name">Event name</label>
				<input
					id="event-name"
					bind:value={eventName}
					type="text"
					placeholder="Enter Event name"
					class="py-4 pl-5 pr-24 bg-transparent border border-borderclr"
				/>
			</div>
			<!-- Description -->
			<div class="flex flex-col my-4">
				<label for="event-name">What is the event about ?</label>
				<textarea
					id="event-name"
					bind:value={eventDescription}
					placeholder="Enter Event description"
					class="py-4 pl-5 pr-24 bg-transparent border border-borderclr"
				/>
			</div>
			<!-- Event Date -->
			<div class="flex flex-col my-4">
				<label for="event-date">When is the event ?</label>
				<input
					id="event-date"
					bind:value={eventDate}
					type="date"
					placeholder="Enter Event description"
					class="py-4 pl-5 pr-24 bg-transparent border border-borderclr"
				/>
			</div>
			<!-- Input for guests -->
			<div class="flex my-4">
				<div class="flex flex-col flex-1 guest-container">
					<div class="flex flex-col">
						<!-- Guest name -->
						<div class="flex flex-col flex-1">
							<label for="guest">Enter guest name</label>
							<input
								type="text"
								bind:value={guestName}
								placeholder="Enter guest name"
								class="py-4 pl-5 pr-24 bg-transparent border border-borderclr mb-4"
							/>
						</div>
						<!-- Guest Photo -->
						<div class="flex flex-col flex-1">
							<label for="file_input">Upload Guest Photo</label>
							<input
								id="file_input"
								type="file"
								on:change={handleFileInputChange}
								class="py-4 pl-5 pr-24 bg-transparent border border-borderclr mb-4"
							/>
						</div>
					</div>
					<!-- designation -->
					<div class="flex flex-col">
						<label for="designation">Enter guest designation</label>
						<input
							id="designation"
							type="text"
							bind:value={guestDesignation}
							placeholder="Enter guest designation"
							class="py-4 pl-5 pr-24 bg-transparent border border-borderclr mb-4"
						/>
					</div>
				</div>
			</div>

			<button
			disabled={loading}
			  on:click={createEvent}
				class="py-2 px-8 bg-white text-black mt-8 disabled:bg-white/25 disabled:cursor-not-allowed"
				>{loading?"Creating":"Create"}</button
			>
		</div>
	</div>
</main>
