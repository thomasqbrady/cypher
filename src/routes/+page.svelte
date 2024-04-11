<script lang="ts">
	import { RangeSlider } from '@skeletonlabs/skeleton';
	import { IconLock, IconLockOpen2, IconSparkles } from '@tabler/icons-svelte';

	let alphabet: string[] = [];
	let cypherKey = 1;
	let textAreaText = '';
	$: cypherKeyLetter = String.fromCharCode(64 + cypherKey);

	for (let i = 0; i < 26; i++) {
		alphabet.push(String.fromCharCode(65 + i));
	}

	function encrypt() {
		let encryptedText = '';
		const characters = textAreaText.split('');
		characters.map((character) => {
			let charCode = character.charCodeAt(0);
			if ((charCode >=65 && charCode <= 90) || (charCode >= 97 && charCode <= 122)) {
				encryptedText += String.fromCharCode(charCode + cypherKey - 1);
			} else {
				encryptedText += character;
			}
		});
		navigator.clipboard.writeText(encryptedText);
		textAreaText = '';
	}

	function decrypt() {
		let decryptedText = '';
		const characters = textAreaText.split('');
		characters.map((character) => {
			let charCode = character.charCodeAt(0);
			let delta = cypherKey - 1;
			if ((charCode >=(65+delta) && charCode <= (90+delta)) || (charCode >= (97+delta) && charCode <= (122+delta))) {
				decryptedText += String.fromCharCode(charCode - (delta));
			} else {
				decryptedText += character;
			}
			textAreaText = decryptedText;
		});
	}

	function reset() {
		textAreaText = '';
	}
</script>
<div class="p-4">
	<h1 class="h1 text-6xl">Caesar</h1>
	<button type="button" class="btn-icon bg-gradient-to-br variant-gradient-primary-secondary fixed top-4 right-4" on:click={reset}><IconSparkles/></button>

	<RangeSlider class="mb-8" name="cypherkey" bind:value={cypherKey} min={1} max={26} step={1} ticked><span class="text-2xl">Key: {cypherKeyLetter}</span></RangeSlider>

	<textarea class="textarea text-2xl" rows="4" placeholder="Enter your message here" bind:value={textAreaText} />

	<div class="text-center my-4">
		<button type="button" class=" mr-4 btn bg-gradient-to-br variant-gradient-primary-secondary" on:click={encrypt}>
			<span>Encrypt</span>
			<span><IconLock/></span>
		</button>
		<button type="button" class="btn bg-gradient-to-br variant-gradient-primary-secondary" on:click={decrypt}>
			<span>Decrypt</span>
			<span><IconLockOpen2/></span>
		</button>
	</div>
</div>