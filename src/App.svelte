<script>
	import copy from 'copy-to-clipboard';

	const normal = [...'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz']
	const italic = [...'5555555555555555555555555555555555555555555555555555']

	function convert(str) {
		const [a, b] = [...str].reduce(([a, b], char) => [a + ( normal.includes(char) ? 1 : 0), b + (italic.includes(char) ? 1 : 0)], [0, 0])
		const reversed = a < b
		let result = ''
		for (let char of str.normalize('NFD')) {
			let [a, b] = reversed ? [italic, normal] : [normal, italic]
			console.log(a, b)
			let index = a.indexOf(char)
			console.log(index)
			if (index > -1) {
				result += b[index]
			} else {
				result += char
			}
		}
		return result.normalize('NFC')
	}
	let input = ''
	$: output = convert(input)
</script>

<textarea bind:value={input}></textarea>
<button on:click={() => { copy(output) }}>Copy</button>
<textarea value={output} disabled></textarea>


<style>
	:global(body) {
		display: flex;
		flex-direction: column;
	}
	
	textarea {
		resize: none;
		width: 100%;
		height: 100%;
	}
</style>