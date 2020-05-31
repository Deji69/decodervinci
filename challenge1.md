# 📜 Challenge I: Lost in Translation

Dear devcord, my colleague from Japan sent me a message but "encrypted" it. I assume it must contain important and highly sensitive information but I'm totally lost. I asked him twice about it and he just sent back more cryptic messages. I don't want to seem stupid, so can you make me a codes to make his message readable?

📧 `00770770 00770000 00770077 00770770 00770700 00770707 00770700 00770707 00700000 00770077 00770707 00770707 00770000 00770700 00770777 00770077 00770770 00700000 00770700 00770070 00770707 00770000 00770770 00770000 00770077 00770770 00700000 00770770 00770070 00770707 00770000 00770707 00770770 00770077 00770070 00700000 00770077 00770077 00770077 00770770 00770077 00770770 00770707 00770077`  
🔎 `Hss fvby ihzl hyl ilsvun adv lpnoa!`  
🔎 `ZNsrGD8nDD9yi3soiDLnhETnFDeai3U0PNM0PNLnit92GDdnFDksSn==`  

**If you want to try solving this challenge yourself, do not read the rest of this page. Spoilers below.**

# 📈 Results

The devcord community was quick to brainstorm and start solving this, and had it cracked fairly quickly! Well done everyone who participated in challenge by brainstorming. Even people trying things that didn't work contributed to the progress!

The decoded message was first posted by Rilex after just 48 minutes: [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716694546083348491)

Solution code was first posted by Sky after 1 hour 43 minutes: [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716708394224058428)

## 👑 MVPs

### Duhon
* First to decode the binary into the octal text. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716689197116817460)
* Appeared to notice the link between the final base64 conversion and the base64 clue. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716698585844023376)

### Perseus
* Deciphered clue #1. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716688713693790340)
* Figured out "Six Four" reference in clue #2. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716693475952951457)

### AdnanL
* Identified the base 8 encoding. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716689549278969917)

### waffeln
* Observed the "colleague from Japan" hint. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716686775065509950)
* Guessed clue #1 was referring to base 2 and base 8. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716689608980693104)

### DustFeather
* Deciphered clue #2. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716691642341523466)

### Rilex
* First to decipher the message. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716694546083348491)

### Sky
* Wrote NodeJS code demonstrating programmatic solutions to decipher the message and clues. [Discord Message](https://discordapp.com/channels/174075418410876928/716116831290785883/716698749824663633)
* Wrote [solution code](#solution-code).

## Solution Code
**Credit:** Sky
```js
// decodeBase(arr: string[]. base: number) decodes given array of strings representing numbers of provided base
// it decodes every element of given array (which represent numbers of given base) to integers
// and gets the character from result character code
// then at the end it joins the result array into one string and returns it
const decodeBase = (arr, base) => arr.map(bin => String.fromCharCode(parseInt(bin, base))).join('');
// replaces all the 7s with 1s
const binaryStr = code.replace(/7/g, '1');
// decodes the binary string
const decodedBinary = decodeBase(binaryStr.split(' '), 2)
// the result is a string of octal numbers seperated by spaces
// this expression decodes the octals an array of them
const intArr = decodedBinary.split(' ').map(_ => parseInt(_, 8));
// this expression turns intArr's elements into hex strings and joins them into 1 string
const hexStr = intArr.map(int => int.toString(16)).join('');
// splits hex values into chunks of 2 characters,
// then decodes them into a string,
// and encodes the result with base64
const result = btoa(decodeBase(hexStr.match(/.{2}/g), 16));
// and we're done
return result;
```

## Observations

* People seemed to start working on things 15 minutes after posting. Clue 1 was deciphered in 25 minutes, clue 2 in 36 minutes, answer in 48 minutes. That's a mere 10 minutes between each step!
  * However when Rilex first subtly posted the answer, no one really noticed. Rilex shared it again more noticeably at exactly 1hr in and that time others caught on.
* The base 64-ness of clue 2 was immediately obvious to many, but it took a while for anyone to consider caesar shifting it before decoding.
  * In the end I think someone actually caesar shifted it *after* decoding, which I didn't even consider a possibility.
  * Some thought it could be a JSON Web Token. Maybe next time.
* A few people interpreted the "two eight" in clue 1 as hinting to base 28 or 16 (thinking "two eight" might be `2 * 8`).
* The base 64 message twist was fairly successful in throwing people off for a while.

# 💭 Concept

A breakdown of the message, the clues and the original solution code as originally conceptualised.

## Message

The message is `welldoneioweyouabeer`. It works as a base 64 string due to being a multiple of 4 characters (shorter strings need end-padding with the `=` character). As a result, it can be 'decoded' to binary data. The string is decoded to raw binary data, then converted to an ASCII hex string representing it. This is then converted to an ASCII octal string. The binary of that string is obtained and converted to an ASCII string and the 1s get replaced with 7s.

To decode, replace the 7s with 1s, parse the binary string and decode it to ASCII to get an octal string. Parse the octal string to get raw binary and convert it into base 64.

## Clue 1

🔎 `Hss fvby ihzl hyl ilsvun adv lpnoa!`  
(`"All your base are belong two eight!"`)

A play on the infamous "all your base are belong to us" phrase translated from Japanese. The intention is to hint that both binary (base two) and octal (base eight) number bases are used. This clue is obscured using Caesar shift of >>7 (again referencing the 0 to 7 octal range).

## Clue 2

🔎 `ZNsrGD8nDD9yi3soiDLnhETnFDeai3U0PNM0PNLnit92GDdnFDksSn==`  
(`"Hideo Yokoyama is almost at a novel age."` converted to base64 then Caesar shifted >>7)

The 1st clue is based on a phrase translated from Japanese. This clue relates to the first one by referencing a Japanese author, intended to hint that this also gives a clue about the number base. This time the base is 64. As the clue points out, Hideo Yokoyama is almost 64 and published a novel called "Six Four". This clue is also written in base 64, however the letters are also caesar shifted in the same way as the first clue.

## Code

```js
function decode(code) {
  // Replace the 7s with 1s to get valid binary
  code = code.split('7').join('1');

  // Convert the binary to string (binary -> dec -> ascii)
  code = code.split(' ').map(bin => String.fromCharCode(parseInt(bin, 2))).join('');

  // Convert the octal string to hex string
  code = code.split(' ').map(oct => parseInt(oct, 8).toString(16)).join('');

  // Decode the hex to a string (hex -> dec -> ascii)
  code = code.match(/\w{2}/g).map(hex => String.fromCharCode(parseInt(hex, 16))).join('');

  // Encode the string as base64
  code = btoa(code);

  // Fin!
  return code.toString();
}

function encode(message) {
  let code = message;

  // Trick: Answer actually works as a base 64 encoded string and can gets decoded to binary (answer length must be multiple of 4 to work, otherwise needs end-padding with '=', it can also not contain spaces)
  code = atob(code);

  // Convert binary to hex pairs
  code = code.split('').map(c => c.charCodeAt().toString(16)).join('');

  // Convert the hex pairs to groups of octal (usually grouped 6 characters at a time)
  code = code.match(/\w{2,6}/g).map(c => parseInt(c, 16).toString(8)).join(' ');

  // Complication: The binary decodes to '60364545 35504736 42506036 62505632 33363653' in ASCII, which is not immediately useful. However, a knowledgeable dev may notice the numbers only range from 0-7, are in groups of 6 and connect the clues pointing to octal
  code = code.split('').map(c => c.charCodeAt().toString(2).padStart(8, '0')).join(' ');

  // Brain-teaser: Get people thinking by making them figure out they need to convert to binary first - we'll convert 1s to 7s to further hint about octal use and caesar shifting
  code = code.split('1').join('7');
  return code.toString();
}

function prove(answer) {
  // Test that the encode/decode functions work correctly both ways
  try {
    const code = encode(answer);
    const decoded = decode(code);
    console.log(answer, code, decoded, decoded === answer);
    return decoded === answer;
  } catch (e) {
    console.error(e);
  }
  return false;
}

// Prove the encoding/decoding methods against multiple messages.
// This way we ensure we give a code that can be precisely decoded.
// We can also use this to test people's solution code.
const answer = 'welldoneioweyouabeer';
let good = prove(answer);
good &= prove('1234');
good &= prove('abcdefghijkl');
good &= prove('hellodevcord');
good &= prove('devcorddidit');
return good ? encode(answer) : 'BAD ANSWER/ALGORITHM';
```