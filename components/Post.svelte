<script>
  export let imagePreview = "";
  export let caption = "";

  function handleImageUpload(event) {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = () => {
        if (typeof reader.result === "string") {
          imagePreview = reader.result;
        }
      };
      reader.readAsDataURL(file);
    }
  }
</script>

<input type="file" accept="image/*" on:change={handleImageUpload} />

{#if imagePreview}
  <img src={imagePreview} alt="Preview" class="preview" />
{/if}

<textarea
  rows="3"
  placeholder="Write a caption about your day..."
  bind:value={caption}
></textarea>

<style>
  .preview {
    display: block;
    max-width: 200px;
    margin: 0.8rem 0;
    border-radius: 8px;
  }
  textarea {
    width: 100%;
    padding: 0.6rem;
    border-radius: 8px;
    border: 1px solid #ccc;
  }
</style>
