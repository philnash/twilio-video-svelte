<script>
  import { onMount } from "svelte";
  import Video from "twilio-video";
  import Participant from "./Participant.svelte";
  export let token;
  export let roomName;
  export let destroyToken;
  let room = null;
  let participants = [];

  const disconnect = () => {
    if (room) {
      room.removeAllListeners();
      room.disconnect();
      room = null;
      participants = [];
    }
  };

  const leaveRoom = () => {
    disconnect();
    destroyToken();
  };

  onMount(async () => {
    room = await Video.connect(token, { name: roomName });
    participants = Array.from(room.participants.values());
    room.on("participantConnected", participant => {
      participants = [...participants, participant];
    });
    room.on("participantDisconnected", participant => {
      participants = participants.filter(p => p !== participant);
    });
    return () => {
      disconnect();
    };
  });
</script>

{#if room !== null}
  <button on:click={leaveRoom}>Leave room</button>

  <Participant participant={room.localParticipant} />

  <h2>Remote Participants</h2>
  {#each participants as participant}
    <Participant {participant} />
  {/each}
{/if}
