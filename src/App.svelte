<script>
	import copy from 'copy-to-clipboard';

	const normal = [...'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz']
	const italic = [...'𝘈𝘉𝘊𝘋𝘌𝘍𝘎𝘏𝘐𝘑𝘒𝘓𝘔𝘕𝘖𝘗𝘘𝘙𝘚𝘛𝘜𝘝𝘞𝘟𝘠𝘡𝘢𝘣𝘤𝘥𝘦𝘧𝘨𝘩𝘪𝘫𝘬𝘭𝘮𝘯𝘰𝘱𝘲𝘳𝘴𝘵𝘶𝘷𝘸𝘹𝘺𝘻']

	function convert(str) {
		const [a, b] = [...str].reduce(([a, b], char) => [a + ( normal.includes(char) ? 1 : 0), b + (italic.includes(char) ? 1 : 0)], [0, 0])
		const reversed = a < b
		let result = ''
		for (let char of str.normalize('NFD')) {
			let [a, b] = reversed ? [italic, normal] : [normal, italic]
			let index = a.indexOf(char)
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