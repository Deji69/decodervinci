# 📜   **<u>De</u>  <u>Cod</u>e<u>r</u>  <u>V</u>inci**  

Dear devcord, your combined skills are needed to decrypt secret messa**g**es! All messages will be a **h**um**a**n readable "English" message, keyphrase, etc. **o**nce dec**od**ed. It should be fair**l**y ob**v**io**u**s to t**e**ll when you have the decoded message. Encryption methods may involve di**f**ferent encryption or hashing algorithms or other tri**ck**s, and anything that the average devcord **u**ser may be expected to k**n**ow.

<details>🔍 Clues will be given, but may also need to be cracked. The method required to crack a clue may also be a clue. 🐱‍👤</details>

`^..^^^^.^..^^..`  
`.^^....^.^^..^^`

** **
## Guidelines
  * MVPs will be 👑'd and [featured](challenge1.md) on the result post and page, as well as awarded kudos (devcord points). MVPs are generally anyone who posts anything true and helpful in progressing or solving the challenge, but are picked at my own discretion.
  * The person who first shows the decoded result will be crowned as MVP.
  * The person who first posts solution code in JavaScript (Node or Front End) or PHP will be crowned as MVP, so long as the result can be quickly and easily verified by myself.
  * Solution code must demonstrate the proper steps to solve the puzzle. For example, you should not do anything like code.replace('414243', 'abc') to convert hex to text. Your code may be tested against multiple other codes which are similarly encrypted so make it as flexible as you can!
  * Feel free to Google and steal use code you find online in your solution code.
  * New challenges should arrive each Sunday at 17:00 UTC, with any luck, although I'm currently organising these alone, so delays or changes may occur.
  * If you have any doubts or questions about the challenge(s), feel free to ping me on the server, but be mindful that answers are will be made at my own convenience and discretion (I won't spoil clues unless I feel it's absolutely necessary)..

** **
## [__Companion page__](/decodervinci/)  
  If your solution code uses front-end JavaScript, you may validate it on this page. Enter the encoded input into the 'Input code' field, write your decoding code in the editor and press `decode(code);`. The result is displayed below. The result will work for both plain text and HTML DOM elements. Given acceptable code, the decoded answer should appear as the result. If your code returns a promise, it will be resolved using `await`.

** **
## Hints
  * Pay close attention to every detail of the codes. Try and identify formats, encodings, number systems, etc.
  * Info and online decoders can be found online for most things.
  * Riddles may come into play. I apologise in advance :stuck_out_tongue: 
  * Knowledge of common and obscure web dev trivia may prove useful.
  * Obviously knowing JS is a big advantage, but Google/StackOverflow skills are more important. You can find most of the answers you need without too much knowledge of the language.

** **
## Exempli gratia
  Given the code `6 8 6 5 6 c 6 c 6 f`, savvy devs may notice every character is a valid hex char, just separated by spaces between the digits. The first step would be to remove those spaces so it can be parsed. The next step would be to decode the hexadecimal. Super savvy devs might already be able to tell that the pairs are in ASCII letter range. Converting the hex to ASCII characters gives us the answer "hello". The JavaScript solution would therefore be:
```js
// Remove spaces between chars
code = code.split(' ').join('');
// Match hex char pairs, replace each pair with the string representation
code = code.match(/\w{2}/g).map(
    hex => String.fromCharCode(parseInt(hex, 16))
).join('');
// Return the decoded string
return code;
```
If you went to the [companion page](https://deji69.github.io/decodervinci/) with that code, you would see the answer "hello" in the output.  
Don't worry if you couldn't tell the hex chars were in ASCII range, or even if they were ASCII chars. Experiment and use your intuition, you'll eventually get one step closer to figuring things out.
