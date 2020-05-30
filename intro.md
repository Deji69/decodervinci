# 📜   **<u>De</u>  <u>Cod</u>e<u>r</u>  <u>V</u>inci**  

Dear devcord, your combined skills are needed to decrypt secret messages! All messages will be a human readable "English" message, keyphrase, etc. once decoded. It should be fairly obvious to tell when you have the decoded message. Encryption methods may involve different encryption or hashing algorithms or other tricks, and anything that the average devcord user may be expected to know.

<details>🔍 Clues will be given, but may also need to be cracked. The method required to crack a clue may also be a clue. 🐱‍👤</details>

** **
## Rules
  * Answers must be given as front end JavaScript code which runs on the companion page and returns the decoded message.
  * Answer code must demonstrate the proper steps to solve the puzzle. For example, you should not do anything like `code.replace('414243', 'abc')` to convert hex to text. Your code should be ready to decode multiple codes encrypted with similar methods.
  * Feel free to Google and ~~steal~~ use code you find online in your answer code.
  * Clues and answers can be shared and discussed freely in the channel.
  * Collaboration is highly encouraged! MVPs will be crowned as well as those who post code answers. This is more of group effort to find the answer than a competition, but feel free to get competitive if you want.
  * If you have any doubts or questions about the challenge(s), feel free to ping @Deji#4519, but be mindful that answers are will be made at his own convenience and discretion (he will not spoil clues unless he feels it is absolutely necessary).

** **
## [__Companion page__](/decodervinci/)  
  The purpose of this page is to validate your answer code. Enter your code into the editor and press `decode(code);`. The result is displayed below. Given acceptable code, the decoded message should appear as the result. You are of course free to use whatever editors and tools you want, so long as you end up with code that works on that page. The editor provided there is fairly capable, so you may also code your solution there, just be careful not to refresh the page (your submissions should always work without needing to refresh).

** **
## Hints
  * Pay close attention to every detail of the codes. Try and identify formats, encodings, number systems, etc.
  * Info and online decoders can be found online for most things.
  * Riddles may come into play. I apologise in advance :stuck_out_tongue: 
  * Knowledge of common and obscure web dev trivia may prove useful.
  * Obviously knowing JS is a big advantage, but Google/StackOverflow skills are more important. You can find most of the answers you need without too much knowledge of the language.

** **
## Example
  Given the code `6 8 6 5 6 c 6 c 6 f`, savvy devs should be able to see that every character is a valid hex char, just separated by  spaces between each digit. The first step would be to remove those spaces so it can be parsed. The next step would be to decode the hexadecimal. Super savvy devs might already be able to tell that the pairs are in ASCII letter range. Converting the hex to ASCII characters gives us the answer "hello". The JavaScript solution would therefore be:
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
If you went to the [companion page](https://deji69.github.io/decodervinci/) and submitted that code, you would see the answer in the output.  
Don't worry if you couldn't tell the hex chars were in ASCII range, or even if they were ASCII chars. Experiment and use your intuition, you'll eventually get one step closer to figuring something out.
