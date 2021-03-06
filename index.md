# 📜   **<u>De</u>  <u>Cod</u>e<u>r</u>  <u>V</u>inci**  

<link rel="stylesheet" href="./styles/codemirror.css">
<style>
	body { font-size:18px; }
	pre { margin: 0; }
	button { background:#DDD !important; width: 200px; padding: 6px 0; }
	.wrapped,.wrapped>code { white-space: normal !important; }
</style>
<script src="codemirror.js"></script>
<script src="./modes/javascript.js"></script>
<script>
console.log('Hello hacker!');
const urlParams = new URLSearchParams(location.search);
function updateUrlGet(name, val) {
	urlParams.set(name, val);
	const url = location.protocol + "//" + location.host + location.pathname + '?' + urlParams.toString();
	window.history.pushState({path:url}, '', url);
}
let code = '6 8 6 5 6 c 6 c 6 f';
document.addEventListener('DOMContentLoaded', () => {
	const runButton = document.getElementById('run');
	const resultCode = document.getElementById('result-code');
	const codeEl = document.getElementById('code');
	const editor = CodeMirror.fromTextArea(codeEl, {lineNumbers: true});
	const assignValueEl = document.getElementById('assign-value');
	const codeInput = document.getElementById('code-input');
	codeInput.placeholder = code;
	editor.on('changes', _ => updateUrlGet('src', btoa(editor.getValue())));
	codeInput.addEventListener('input', e => {
		code = e.target.value;
		assignValueEl.innerText = '"'+code+'"';
		updateUrlGet('input', code);
	});
	runButton.addEventListener('click', () => {
		resultCode.innerHTML = '';
		const src = 'return (async function(){'+editor.getValue()+';})();';
		const func = new Function('code', src);
		func(code).then(res => {
			console.log(res);
			if (typeof(res) === 'string')
				resultCode.innerText = res;
			else if (res instanceof Node)
				resultCode.appendChild(res);
			else {
				resultCode.innerHTML = '<b>Invalid result type, string or Node expected</b>';
				return;
			}
		});
	});
	if (urlParams.has('src')) editor.getDoc().setValue(atob(urlParams.get('src')));
	if (urlParams.has('input')) {
		codeInput.value = urlParams.get('input');
		codeInput.dispatchEvent(new Event('input', { bubbles: true }))
	}
});
</script>

Check out the [introduction](/decodervinci/intro).

Input code: <input id="code-input" type="string" placeholder="6 8 6 5 6 c 6 c 6 f">

<pre class="cm-s-default"><code class="wrapped"><span class="cm-keyword">const</span> <span class="cm-variable-2">code</span> <span class="cm-operator">=</span> <span id="assign-value" class="cm-string">"<script>document.write(code)</script>"</span>;</code><code>
<span class="cm-keyword">async</span> <span class="cm-keyword">function</span> <span class="cm-variable-2">decode</span>(<span class="cm-variable-2">code</span>) {<textarea id="code">// edit me!
// Remove spaces between chars
code = code.split(' ').join('');
// Match hex char pairs, replace each pair with the string representation
code = code.match(/\w{2}/g).map(
    hex => String.fromCharCode(parseInt(hex, 16))
).join('');
// Return the decoded string
return code;</textarea>}
</code></pre>
<button id="run" class="CodeMirror cm-s-default" type="button"><span class="cm-keyword">await</span> <span class="cm-variable-2">decode</span>(<span class="cm-variable-2">code</span>);</button>
<div class="result wrapped">
	<h3>Output:</h3>
	<pre><code id="result-code"></code></pre>
</div>