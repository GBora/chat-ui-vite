<script lang="ts">
  const doPost = async () => {
    // loading = true;

    let input = document.querySelector('input[type="file"]')

    // let data = new FormData()
    // data.append('file', input.files[0])

    console.log(input.files[0])

    const res = await fetch('http://127.0.0.1:8000/upload', {
        method: 'POST',
        headers: new Headers({
            "Content-Type": "application/json",
            "accept": "application/json"
        }),
        body: JSON.stringify({
          uploaded_file: input.files[0]
        })
    })

    let response = await res.json();

    console.log(response);

    // loading = false;
}
</script>
<div>
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
</div>