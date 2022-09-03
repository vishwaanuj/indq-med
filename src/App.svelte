<script>
  import { onMount } from "svelte";
  import { fade } from "svelte/transition";
  import { longpress } from "./action";
  import { LazyImage } from "svelte-lazy-image";
  import LOGO from "./logo-small.png";
  import SubLogo from "./sublogo.png";
  import { loadingController } from "@ionic/core";
  import Preview from "./preview.svelte";
  const imageModules = import.meta.glob("./images/*.jpg");
  let imgArray = [];
  let loaded = false;
  let show_preview = false;
  let PREVIEW_PATH;
  var showLoading = async () => {
    const loading = await loadingController.create({
      message: "Loading...",
      duration: 100,
      cssClass: "custom-loading",
    });

    loading.present();
  };

  function preview(path) {
    //alert(window.location.pathname);
    show_preview = true;
    PREVIEW_PATH = path;
  }

  function share(data) {
    navigator.share();
  }
  function load_images() {
    for (const modulePath in imageModules) {
      let loc = window.location.pathname + modulePath;
      imgArray.push(modulePath);
    }
    console.log(window.location.pathname);
    loaded = true;
  }
  let pressed;
  onMount(() => {
    load_images();
  });
  // code here
</script>

<ion-header style="background-color:black">
  <ion-toolbar>
    <ion-title style="background-color:black">
      <img class="logo" src={LOGO} alt="MedExpo2022" />
    </ion-title>
  </ion-toolbar>
</ion-header>

<div in:fade class="ion-padding">
  <div id="container">
    <img class="sublogo" src={SubLogo} alt="MedExpo2022" />

    <div class="subheader">
      <strong>MedTech Gallery Expo 2022</strong>
    </div>

    <div>
      <!-- promise is pending -->

      <!-- promise was fulfilled -->

      {#if loaded}
        <!-- content here -->
        <ion-row>
          {#each imgArray as image}
            <!-- content here asd-->

            <ion-col
              on:click={() => {
                preview("./src/" + image);
              }}
              size={4}
            >
              <LazyImage
                src="./src/{image}"                placeholder="INDQ Medtech Expo 2022"
                alt={image}
              />
            </ion-col>
          {/each}
        </ion-row>
        {#if show_preview}
          <div class="preview_box">
            <div
              on:click={() => {
                show_preview = false;
              }}
              style="background-color: black;"
            >
              close
            </div>
            <Preview img_src={PREVIEW_PATH} />
          </div>
          <!-- content here -->
        {:else}
          <!-- else content here -->
        {/if}
      {:else}
        <!-- else content here -->
        {showLoading()}
      {/if}
    </div>
  </div>
</div>

<style>
  ion-title {
    text-align: center;
    padding: 10px;
  }

  .logo {
    max-height: 30px;
  }

  .sublogo {
    max-height: 230px;
    min-height: 170px;
    min-width: 100px;
    border-radius: 10px;
  }
  #container {
    text-align: center;
    height: 100vh;
  }

  .subheader {
    padding: 12px;
    border:solid .5px gray;
    border-radius: 10px;
    font-size: 21px;
    margin: 10px;

  }

  .preview_box div {
    position: fixed;
    top: 7vh;
    padding: 10px;
    z-index: 2;
    left: 0;
  }
</style>
