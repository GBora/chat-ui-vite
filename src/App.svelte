<script lang="ts">
      import { type IWindow } from "./interfaces/window";
      import { TabContent, TabPane } from 'sveltestrap';

      let text="";
      let loading = false;
      let response = { 
          answer: '',
          sources: []
      };
      let audioMessage = '';
  
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

      const listen = async () => {
        const {webkitSpeechRecognition} : IWindow = <IWindow><unknown>window;
        const recognition = new webkitSpeechRecognition();

        recognition.onstart = () => {
            audioMessage = 'Listening ...'
        }
        
        recognition.onresult = (e) => {
            console.log(e);
            let transcript = e.results[0][0].transcript;
            text = transcript;
            audioMessage = '';
        }

        recognition.start();
      }
  </script>
    
  <div class="container mx-auto w-50 mt-5">

    <TabContent>
        <TabPane tabId="bravo" tab="Questions" active>
            {#if !loading}
            <div class="mb-3 mt-3">
                <textarea class="form-control" rows="5" cols="5" bind:value={text}></textarea>
            </div>
            <div class="d-flex mb-3">
                <div class="col-5">
                    <button class="btn btn-primary btn-block w-100" on:click={doPost}>
                        Ask
                    </button>
                </div>
                <div class="col-2">
    
                </div>
                <div class="col-5">
                    <button class="btn btn-success btn-block w-100" on:click={listen}>
                        Speak
                    </button>
                </div>
            </div>
            <div class="mb-3 text-center">
              {audioMessage}
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
        </TabPane>
        <TabPane tabId="charlie" tab="Sources">
 
        </TabPane>
    </TabContent>
  </div>
  