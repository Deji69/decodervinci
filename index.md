<html>
	<head>
		<title>De Coder Vinci</title>
		<link rel="stylesheet" href="./styles/codemirror.css">
		<style>
			body { font-size:18px; }
			pre { margin: 0; }
			button { background:#DDD !important; width: 150px; padding: 6px 0; }
			.wrapped { white-space: normal !important; }
		</style>
		<script src="codemirror.js"></script>
		<script src="./modes/javascript.js"></script>
		<script>
			console.log('Hello hacker!');
			const code = '00770077 00770707 00770077 00770770 00770707 00770777 00770077 00770700 00700000 00770707 00770000 00770707 00770077 00770077 00770707 00770077 00770707 00700000 00770700 00770070 00770077 00770707 00770700 00770070 00770707 00770707';
			document.addEventListener('DOMContentLoaded', () => {
				const runButton = document.getElementById('run');
				const resultCode = document.getElementById('result-code');
				const codeEl = document.getElementById('code');
				const editor = CodeMirror.fromTextArea(codeEl, {
					lineNumbers: true,
				});
				runButton.addEventListener('click', () => {
					const src = editor.getValue();
					const func = new Function('code', src);
					const res = func(code);
					console.log(res);
					if (typeof(res) === 'string')
						resultCode.innerText = res;
					else
						resultCode.innerHTML = '<b>Invalid result type, string expected</b>';
				});
			});
		</script>
	</head>
	<body>
		<pre class="cm-s-default wrapped"><code><span class="cm-keyword">const</span> <span class="cm-variable-2">code</span> <span class="cm-operator">=</span> <span class="cm-string">"<script>document.write(code)</script>"</span>;</code></pre>
		<pre class="cm-s-default"><code>
<span class="cm-keyword">function</span> <span class="cm-variable-2">decode</span>(<span class="cm-variable-2">code</span>) {<textarea id="code">// edit me!
return code + ' is the code that needs to be cracked';</textarea>}
		</code></pre>
		<button id="run" class="CodeMirror cm-s-default" type="button">decode(<span class="cm-variable-2">code</span>);</button>
		<div class="result wrapped">
			<h3>Result:</h3>
			<pre><code id="result-code"></code></pre>
		</div>
	</body>
</html>