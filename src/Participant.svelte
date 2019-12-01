<script>
  import { onMount } from "svelte";
  export let participant;

  let videoElement;
  let audioElement;
  let videoTrack = null;
  let audioTrack = null;

  const trackSubscribed = track => {
    if (track.kind === "video") {
      videoTrack = track;
      track.attach(videoElement);
    } else {
      audioTrack = track;
      track.attach(audioElement);
    }
  };

  const trackUnsubscribed = track => {
    if (track.kind === "video" && track === videoTrack) {
      videoTrack.detach();
      videoTrack = null;
    } else {
      audioTrack.detach();
      audioTrack = null;
    }
  };

  onMount(() => {
    videoTrack = Array.from(participant.videoTracks.values())[0];
    if (typeof videoTrack !== "undefined") {
      videoTrack.attach(videoElement);
    }
    audioTrack = Array.from(participant.audioTracks.values())[0];
    if (typeof audioTrack !== "undefined") {
      audioTrack.attach(audioElement);
    }
    participant.on("trackSubscribed", trackSubscribed);
    participant.on("trackUnsubscribed", trackUnsubscribed);

    return () => {
      participant.removeAllListeners();
      if (videoTrack) {
        videoTrack.detach();
        videoTrack = null;
      }
      if (audioTrack) {
        audioTrack.detach();
        audioTrack = null;
      }
    };
  });
</script>

<div>
  <h2>{participant.identity}</h2>
  <video bind:this={videoElement} />
  <audio bind:this={audioElement} />
</div>
