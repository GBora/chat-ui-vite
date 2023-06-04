<script lang="ts">  
      let text="";
      let loading = false;
      let response = { 
          answer: '',
          sources: []
      };
  
      const doPost = async () => {
          loading = true;
  
          const res = await fetch('http://127.0.0.1:8000/question', {
              method: 'POST',
              headers: new Headers({"Content-Type": "application/json", "accept": "application/json" }),
              body: JSON.stringify({
                  text,
                  language: "en"
              })
          })
  
          response = await res.json()
  
          loading = false;
      }
  </script>
  
  <style>
      .custom-loader {
          width: 100px;
          height: 100px;
          display: grid;
          border:8px solid #0000;
          border-radius: 50%;
          border-color:#198754 #0000;
          animation: s6 1s infinite linear;
      }
  
      .custom-loader::before,
      .custom-loader::after {    
          content:"";
          grid-area: 1/1;
          margin:4px;
          border:inherit;
          border-radius: 50%;
      }
      .custom-loader::before {
          border-color:#0D6EFD #0000;
          animation:inherit; 
          animation-duration: .5s;
          animation-direction: reverse;
      }
      .custom-loader::after {
          margin:16px;
      }
  
      @keyframes s6 { 
          100%{transform: rotate(1turn)}
      }
  </style>
  
  <div class="container mx-auto w-25 mt-5">
      {#if !loading}
          <div class="mb-3">
              <input type="text" class="form-control" bind:value={text}>
          </div>
          <div class="d-flex mb-3">
              <div class="col-5">
                  <button class="btn btn-primary btn-block w-100" on:click={doPost}>
                      <i class="fa-solid fa-magnifying-glass"></i>
                  </button>
              </div>
              <div class="col-2">
  
              </div>
              <div class="col-5">
                  <button class="btn btn-success btn-block w-100">
                      <i class="fa-solid fa-microphone"></i>
                  </button>
              </div>
          </div>
      {/if}
  
      {#if loading}
          <div class="d-flex justify-content-center">
              <div class="custom-loader"></div>
          </div>
      {/if}
  
      {#if response.answer}
          
          <div class="mb-3 p-1 border">
              <div>Answer:</div>
              { response.answer }
          </div>
  
          {#each response.sources as source}
              <div class="mb-3 border">
                  <div>Source:</div>
                  <div class="mt-2">Location: {source.metadata.source}</div>
                  <div class="mt-2">Content:</div>
                  <div class="mt-2">
                      {source.page_content}
                  </div>
              </div>
          {/each}
      {/if}
  </div>
  