<script lang="ts">
let loading = false;
let message = '';
const doPost = async () => {
    loading = true;

    let input = document.querySelector('input[type="file"]');

    let data = new FormData();
    data.append('uploaded_file', input.files[0]);

    const res = await fetch('http://127.0.0.1:8000/upload', {
        method: 'POST',
        body: data
    });

    loading = true;

    let response = await res.json();
    message = response.info;
}
</script>
<div>
    {#if !loading}
    <form method="post" enctype="multipart/form-data" on:submit|preventDefault={doPost}>
        <div class="mt-3">
            <label for="source_file" class="form-label">Choose source file to upload</label>
            <input
                type="file"
                id="source_file"
                name="source_file"
                accept=".txt, .pdf, .doc"
                class="form-control"/>
        </div>
        <div class="mt-3">
            <button class="btn btn-primary" type="submit">Submit</button>
        </div>
    </form>
    {/if}
    {#if loading}
    <div class="d-flex justify-content-center">
        <div class="custom-loader"></div>
    </div>
    {/if}
    {#if message}
    <div>
        {{ message }}
    </div>
    {/if}
</div>
