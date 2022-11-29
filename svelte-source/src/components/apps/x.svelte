<script lang="ts">
    import Apps from "@components/shared/Apps.svelte";
    import {
        canCancel,
        contracts,
        queue,
        startedContracts,
    } from "@store/boosting";
    import { notifications } from "@store/notifications";
    import { fetchNui } from "@utils/eventHandler";
    let topData = {
        title: "ktUj9bg6Bv+flc5UDrVA5uMvCm7LMJXKY2fTk0OBv4o=",
        background: "#8775A4",
        blur: true,
        blurstrength: 15,
        color: "#73638D",
    };

    let currentPage = "Kontrakter";
    let iswaiting: boolean;

    // Kø og alle de der andre funktioner
    function joinQueue() {
        if (iswaiting) return;
        iswaiting = true;
        setTimeout(() => {
            iswaiting = false;
            fetchNui("boosting/queue", {
            status: !$queue,
        }).then((res) => {
            if (res.status === "success") {
              $queue = !$queue;
            }
            notifications.send(res.message, "boosting", 5000);
        });
      }, 2000);
    }

</script>
  
<Apps topdata={topData} appname="x">
    <div class="apps">
        <div class="top">
            <div class="navigation">
                <div class="left">

                    <button
                    class:active={currentPage === "Kontrakter"}
                    on:click={() => {
                      currentPage = "Kontrakter";
                    }}>
                    Kontrakter
                    </button>

                </div>
                <div class="right">

                    <button
                    on:click={joinQueue}
                    class:waiting={iswaiting}
                    disabled={iswaiting}
                    class:active={$queue}
                  >
                    {#if iswaiting}
                      Indlæser
                    {:else}
                      {$queue ? "Forlad Kø" : "Tilslut Kø"}
                    {/if}
                  </button>

                </div>
            </div>
        </div>
    </div>
</Apps>

<style>
    .apps {
        overflow: hidden;
        height: 100%;
    }
    .left,
    .right {
        font-size: 30px;
        gap: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .navigation {
        width: 100%;
        display: flex;
        margin-top: 10px;
        justify-content: space-between;
        align-items: center;
        padding: 10px 100px;
    }
    .top {
        margin-top: 10px;
        display: flex;
        align-items: center;
        flex-direction: column;
    }
    button.active {
        color: rgb(255, 255, 255);
        background-color: rgb(87, 64, 93);
        width: 170px;
        height: 50px;
        padding: 10px;
        border-radius: 5px;
        text-align: center;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    button.waiting {
        cursor: not-allowed;
        color: rgba(255, 255, 255, 0.377);
    }
    button {
        color: rgba(255, 255, 255);
        background-color: rgb(87, 64, 93);
        letter-spacing: 2px;
        font-weight: bold;
        cursor: pointer;
        font-size: 0.8rem;
        text-transform: uppercase;
        border: none;
        width: 170px;
        height: 50px;
        padding: 10px;
        border-radius: 5px;
        text-align: center;
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>
  