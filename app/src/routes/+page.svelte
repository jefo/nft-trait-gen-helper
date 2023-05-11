<script>
	let currentImageIndex = 0;
	let images = [];

	function handleFileUpload(event) {
		const files = event.target.files;
		for (let i = 0; i < files.length; i++) {
			images = [...images, files[i]];
		}
		console.log("files", images);
	}

	function handlePrevious() {
		if (currentImageIndex > 0) {
			currentImageIndex--;
		}
	}

	function handleNext() {
		if (currentImageIndex < images.length - 1) {
			currentImageIndex++;
		}
	}

	function handleSubmit(event) {
		event.preventDefault();
		const plotInput = event.target.plot;
		const colorInput = event.target.color;
		const plot = plotInput.value;
		const color = colorInput.value;
		localStorage.setItem(
			"metadata",
			JSON.stringify([
				...(JSON.parse(localStorage.getItem("metadata")) || []),
				{ plot, color },
			])
		);
	}

	function updateImageInfo() {}

	let imageInfo = '';

	$: {
		const file = images[currentImageIndex];
		console.log("currentImageIndex", currentImageIndex);
		console.log("images", images);
		console.log("file", file);
		if (file) {
			const parentFolderName = file.webkitRelativePath.split("/")[0];
			const fileName = file.name;
			imageInfo = `${parentFolderName} / ${fileName}`;
		}
	}
</script>

<div>
	{#if images[currentImageIndex]}
		<img width="400" height="400" src={URL.createObjectURL(images[currentImageIndex])} />
		<form on:submit={handleSubmit}>
			<label>Plot:</label>
			<input type="text" name="plot" />
			<br />
			<label>Color:</label>
			<input type="text" name="color" />
			<br />
			<button type="submit">Save</button>
		</form>
		<div>{imageInfo}</div>
		<button on:click={handlePrevious}>Previous</button>
		<button on:click={handleNext}>Next</button>
	{:else}
		<div>No images loaded</div>
	{/if}
</div>

<input type="file" webkitdirectory on:change={handleFileUpload} />
