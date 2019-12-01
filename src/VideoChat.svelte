<script>
  import Room from "./Room.svelte";
  let identity = "";
  let roomName = "";
  let token = null;

  const handleSubmit = async () => {
    const data = await fetch("TOKEN_URL", {
      method: "POST",
      body: JSON.stringify({
        identity,
        room: roomName
      }),
      headers: {
        "Content-Type": "application/json"
      }
    }).then(res => res.json());
    token = data.token;
  };

  const destroyToken = () => {
    token = null;
  };
</script>

{#if token !== null}
  <Room {token} {roomName} {destroyToken} />
{:else}
  <form on:submit|preventDefault={handleSubmit}>
    <div>
      <label for="identity">Username:</label>
      <input
        name="identity"
        id="identity"
        type="text"
        required
        bind:value={identity} />
    </div>
    <div>
      <label for="room-name">Room name:</label>
      <input
        name="room-name"
        id="identity"
        type="text"
        required
        bind:value={roomName} />
    </div>

    <button type="submit">Log in</button>
  </form>
{/if}
