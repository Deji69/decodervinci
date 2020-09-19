# Challenge II: Painting Bye Nary

Dear devcord, a little background, we operate a virtual art exhibition, hosting famous works such as "Loner Meesa Squint", "The Seam of Chaos" and "Thy Heartstring". Anyway, our web software was hacked recently and our greatest works were stolen, the virtual canvases have been replaced by the hacker. The only traces of clues we have that may be useful for painting a picture of how the attack happened are messages left in the system by them. Please recover whatever you can of our 3 most prized works. I hope this won't be a super imposition for you!

## 🖼 Painting 1: Loner Meesa Squint

<https://deji69.github.io/decodervinci/challenge2/loner-meesa-squint>

## 🖼 Painting 2: The Seam of Chaos

<https://deji69.github.io/decodervinci/challenge2/the-seam-of-chaos>

## 🖼 Painting 3: Thy Heartstring

<https://deji69.github.io/decodervinci/challenge2/thy-heartstring>

## 🔎 Clues
🔎 #1: 📦 `h:go3t/umIt/r/4pi.Mysmcm8`  
🔎 #2: <https://deji69.github.io/decodervinci/challenge2/clue2>  
🔎 #3: <https://deji69.github.io/decodervinci/challenge2/clue3>  
🔎 #4: <https://deji69.github.io/decodervinci/challenge2/clue4.png>  
🔎 #5: <https://deji69.github.io/decodervinci/challenge2/clue5.png>  
🔎 #6: <https://deji69.github.io/decodervinci/challenge2/clue6.png>

# Solution / Method

## Code

### Painting 1 (Loner Meesa Squint)

Decoder only, the image was created using a separate graphics program.

```js
function decode(code) {
  // Convert the hex string to a binary string
  code = code.match(/\w{2}/g)
    .map(hex => parseInt(hex, 16).toString(2).padStart(8, '0'))
    .join('');
  
  // Create an RGBA buffer
  const arr = new Uint8ClampedArray(48 * 48 * 4);

  for (let i = 0; i < code.length; ++i) {
    // 0s represent white and 1s represent black
    const v = code[i] === '0' ? 255 : 0;
    arr[i * 4] = v;       // Red
    arr[i * 4 + 1] = v;   // Green
    arr[i * 4 + 2] = v;   // Blue
    arr[i * 4 + 3] = 255; // Alpha
  }

  // Create a 48x48 canvas and fill with the pixels
  let canvas = document.createElement('canvas');
  canvas.width = 48;
  canvas.height = 48;
  canvas.getContext('2d').putImageData(new ImageData(arr, 48), 0, 0);
  return canvas;
}
```

### Painting 2 (The Seam of Chaos)
Split an image into two "random noise" images which can be XOR'd together to reveal the source image.
```js
function encode() {
  // Return a promise as we must wait for the source image to load
  return new Promise((resolve, reject) => {
    // Create two canvases to split the source image into
    const canvas1 = document.createElement('canvas');
    const canvas2 = document.createElement('canvas');
    const ctx1 = canvas1.getContext('2d');
    const ctx2 = canvas2.getContext('2d');
    // Put the canvases in a div which we'll return to the output - displaying two separate images
    const div = document.createElement('div');
    div.appendChild(canvas1);
    div.appendChild(canvas2);
    // Load the black and white The Seam of Chaos source image
    const img = new Image();
    img.src = '/the-seam-of-chaos.bmp';
    img.onload = _ => {
      // Set the canvas dimensions and draw the image onto them
      canvas1.width = canvas2.width = img.width;
      canvas1.height = canvas2.height = img.height;
      ctx1.drawImage(img, 0, 0);
      ctx2.drawImage(img, 0, 0);
      
      // Get the image data for the canvases
      const id1 = ctx1.getImageData(0, 0, img.width, img.height);
      const id2 = ctx2.getImageData(0, 0, img.width, img.height);
      const data1 = id1.data;
      const data2 = id2.data;
      
      // Transform the split images, making each one essentially just random noise but together they make a full image
      for (let i = 0; i < data1.length; i += 4) {
        // Randomly pick whether to use a white or black pixel for image 1
        const c1 = Math.random() >= 0.5 ? 255 : 0;
        // If the source pixel is black, make image 2 have the same pixel as image 1
        // If the source pixel is white, make image 2 have an opposite pixel to image 1
        const c2 = data1[i] === 0 ? c1 : 255 - c1;
        // Write the pixels to both images
        data1[i] = c1; data1[i + 1] = c1; data1[i + 2] = c1;
        data2[i] = c2; data2[i + 1] = c2; data2[i + 2] = c2;
      }
      
      // Update the canvases and return the div
      ctx1.putImageData(id1, 0, 0);
      ctx2.putImageData(id2, 0, 0);
      resolve(div);
    };
  });
}
```

### Painting 3 (Thy Heartstring)

#### Clue 3 Encoding
Create a grid of random chars with a hidden string which is revealed by the accompanying CSS.
```js
// CSS class prefix - we need a different one for the compressed/uncompressed grid
const cp = 'u';
// The charset for random chars in the grid
const chars = 'abcdefghijklmnopqrstuvwxyz_/.:';
// Grid dimensions
const [rows, cols] = [14, 33];
// We need to return both the HTML, and the CSS
const div = document.createElement('div');
const html = document.createElement('p');
const css = document.createElement('style');
div.style.color = '#FFF';
div.style.backgroundColor = '#000';
div.appendChild(html);
div.appendChild(css);
// Fill an array with the amount of random characters needed for the grid
let grid = Array.from({length: rows * cols - code.length}, _ => chars[Math.floor(Math.random() * chars.length)]);
// Append blank spaces for the code to the grid array
grid.push(...Array(code.length).fill(' '));
// Now shuffle the grid array with JS's handy built-in method /s
grid = grid.map(v => ({s:Math.random(), v:v})).sort((a, b) => a.s - b.s).map(a => a.v)
// Finally use the grid array to generate the HTML and corresponding CSS
let j = 0;
let dark = [];
grid.forEach((v, i) => {
  const ch = v === ' ' ? code[j++] : v;
  const nl = i > 0 && (i + 1) % cols === 0 ? '\n' : '';
  html.innerHTML += `<span class="${cp}${i}">${ch}</span>${nl}`;
  if (v !== ' ') dark.push(`.${cp}${i}`);
});
css.textContent = dark.join(',') + '{color:#555}';
return div;
```

#### Duck Key Decoding
Check clue 3 can be fully decoded from imgur (and that they do not corrupt the data with compression).
```js
// Return a promise as we must wait for the source image to load
return new Promise((resolve, reject) => {
  // Create a canvas to load the hidden message image into
  const canvas = document.createElement('canvas');
  const ctx = canvas.getContext('2d');
  // Put the canvas and decoded message container in a div which we'll return
  const div = document.createElement('div');
  const p = document.createElement('p');
  div.appendChild(canvas);
  div.appendChild(p);
  // Load the source image embedded with the hidden message
  const img = new Image();
  img.crossOrigin = 'Anonymous'; // shut up CORS!
  img.src = 'https://i.imgur.com/i3UsJQx.png';
  img.onload = _ => {
    // Set the canvas dimensions and draw the image onto them
    canvas.width = img.width;
    canvas.height = img.height;
    ctx.drawImage(img, 0, 0);
    
    // Get the image data for the canvas we're encoding into
    const id = ctx.getImageData(0, 0, img.width, img.height);
    const data = id.data;
    let res = '';
    
    // Replace 4 LSBs in each pixel's red component with the key code
    for (let i = 0; i < data.length; i += 8) {
      p.innerText += String.fromCharCode((data[i] & 0xF) | ((data[i + 4] & 0xF) << 4));
    }
    
    // Return encoded text and check it is identical to input code
    console.log(p.innerText === code);
    resolve(div);
  };
});
```

#### Chaos Image #13 Encoding
Takes a binary string code and creates an image with white pixels representing the 1s and black pixels representing the 0s. Also prefixes an additional hidden message which will show up as coloured pixels. The message characters are stored in the B, G and R components (in that order). If saved as a common 24bit BMP file, this message will be readable in the file's binary due to it storing pixel data in BBGGRR format.
```js
// Return a promise as we must wait for the source image to load
return new Promise((resolve, reject) => {
  // Create a canvas to embed the binary into
  const canvas = document.createElement('canvas');
  const ctx = canvas.getContext('2d');
  const div = document.createElement('div');
  div.appendChild(canvas);
  const img = new Image();
  img.src = '/challenge2/chaos13.png';
  img.onload = _ => {
    // Set the canvas dimensions and draw the image onto them
    canvas.width = img.width;
    canvas.height = img.height;
    ctx.drawImage(img, 0, 0);

    // Get the image data for the canvas we're encoding into
    const id = ctx.getImageData(0, 0, img.width, img.height);
    const data = id.data;
    
    // Hide an extra clue which will look like some coloured pixels before the rest
    const m = 'This image was unlucky\0Mallard:4LSB=53600\0\0';
    let i = 0;
    
    for (let j = 0; j < m.length;) {
      [data[i+2], data[i+1], data[i]] = [m.charCodeAt(j++), m.charCodeAt(j++), m.charCodeAt(j++)];
      i += 3;
      data[i++] = 255; // alpha
    }
    
    let writtenChars = 0;

    // Represent 1s with a white pixel and 0s with a black pixel
    for (; i < data.length; i += 4) {
      const c = writtenChars < code.length ? code[writtenChars++] : '0';
      [data[i], data[i+1], data[i+2]] = c === '1' ? [255,255,255] : [0,0,0];
    }

    // Update the canvases and return the div
    ctx.putImageData(id, 0, 0);
    resolve(div);
  };
});
```

#### Duck Key Encoding
```js
// Takes a source image and creates a version with a hidden key encoded in it
function encodeKey(code, src = '/duck.png') {
  // Return a promise as we must wait for the source image to load
  return new Promise((resolve, reject) => {
    // Create two canvases, the first outputs the original image for comparison
    // The second will contain the hidden code
    const canvas1 = document.createElement('canvas');
    const canvas2 = document.createElement('canvas');
    const ctx1 = canvas1.getContext('2d');
    const ctx2 = canvas2.getContext('2d');
    // Put the canvases in a div which we'll return
    const div = document.createElement('div');
    const p = document.createElement('p');
    div.appendChild(canvas1);
    div.appendChild(canvas2);
    div.appendChild(p);
    // Load the source image to embed with the hidden message
    const img = new Image();
    img.src = src;
    img.onload = _ => {
      // Set the canvas dimensions and draw the image onto them
      canvas1.width = canvas2.width = img.width;
      canvas1.height = canvas2.height = img.height;
      ctx1.drawImage(img, 0, 0);
      ctx2.drawImage(img, 0, 0);
      
      // Get the image data for the canvas we're encoding into
      const id2 = ctx2.getImageData(0, 0, img.width, img.height);
      const data = id2.data;
      let writtenChars = 0;
      
      // Replace 4 LSBs in each pixel's red component with the key code
      for (let i = 0; i < data.length; i += 4) {
        const c = code[writtenChars++].charCodeAt();
        data[i] = (data[i] & 0xF0) | (c & 0xF);
        i += 4;
        data[i] = (data[i] & 0xF0) | ((c >> 4) & 0xF);
      }
      
      // Update the canvases and return the div
      ctx2.putImageData(id2, 0, 0);
      resolve(div);
    };
  });
```

## Clue 1

🔎 📦 `h:go3t/umIt/r/4pi.Mysmcm8`  
(`https://imgur.com/Mm3I4y8` encoded with caesar box cipher)

> `If you want to see your canvases`  
> `My threads require all your wits`  
> `Act fast xor assure more damages`  
> `I might hack your pieces to bits`

The link is encoded using a caesar box cipher (hence the box emoticon). The number of characters is 25, so if you convert this to a grid of 5x5 by inserting a new line every 5 characters, you can read the decoded output from top to bottom, left to right. The first column reads 'https':

```
h:go3
t/umI
t/r/4
pi.My
smcm8
```

The resulting link leads to an image (in keeping with the theme of this challenge) hosted on imgur with the above 'poem'. Each line of the poem contains 32 characters - this doesn't really mean much except that I'm an obsessive perfectionist. Anyway, the first line references **canvases**, since for a front end JavaScript solution these will be required to output 2 of the paintings. The third line has an intentional typo, **xor** instead of *or*, hinting how you can combine 2 images to get the 2nd painting. The final line mentions hacking 'pieces' to **bits**, hinting that the first painting, Loner Meesa Squint, has been converted into a binary bit map, while also relating to the XOR hint.

## Clue 2

🔎 `http://decodervinci.test/challenge2/clue2.html`  

The page contains a series of 64 HTML elements, each containing hex pair. The background colour of each element is a tone of red - the red component being the same as the hex pair value. This hints to the importance of the red component in recovering the key for Thy Heartstring. If the hex is converted to ASCII, the following message is found:

> Duckey clocks out at 6:45 and stargazes from its secret base....

"Stargazes" hints at connection to the "Thy Heartstring" painting, assuming the anagram has already been worked out. The mispelling hints at the usage of the rubber duck image as the key for the Thy Heartstring painting. The length of the message (64 characters - the 4 period ellipses may hint towards this intention) and "secret base" hints towards the base-64 encoding in the data retrieved from the rubber ducky image.

## Clue 3

🔎 `https://deji69.github.io/decodervinci/challenge2/clue3`

Here you can find two grids filled with seemingly random white ASCII characters. The first grid has the heading "Original" and the second has the heading "Compressed". These conceal two URLs:

https://i.imgur.com/VoxHDQ1.png ("original")  
https://i.imgur.com/i3UsJQx.png ("compressed")

Once the necessary CSS key has been found, it can be used to reveal the hidden URLs within the grids by making the random characters dark, leaving only the relevant characters fully white. The latter of these images contains hidden data required to crack Thy Heartstring.

## Clue 4

🔎 https://i.imgur.com/f9I4Ieh.png

A 48x48 black and white image containing nine 16x16 white and black squares. The first row goes white, black, white. The second, white, white, black. The last, black, black, black. This is an XOR pattern. If the colours are the same, the result is black, if the colours are different, the result is white. This hints to the way the *The Seam of Chaos* images need to be combined to get a legible image.

## Clue 5

🔎 https://deji69.github.io/decodervinci/challenge2/clue5.png

The image contains 3 messages occupying the same space. One of the texts is red, one is blue, and one is green. The colours however all blend together... any pixels where the red and green texts overlap produces yellow, any pixels where the green and blue texts overlap produces cyan, and any pixels where the red and blue pixels overlap produces magenta. Any pixels where all 3 overlap turn white. This makes the texts near impossible to fully read, and it's further complicated by a noisy colourful background.

In order to read the texts, simply remove the blue and green channels to view the red text, remove the red and blue channels to read the green text, and remove the red and green channels to read the blue text. This reveals the 3 messages, which are purposely the same length and in a monospace font so they clash - they're also all wrapped with the `#` character, so that at least one character appears as fully white once clashed, giving a hint as to how the image works:

> `#LETTER SWAP, SYNONYM WORD PLAY, ANAGRAM#`  
> `###IISHA LEMMING WAS WORKING FULL TIME###`  
> `##DID YOU FIND THE 13TH CHAOS IMAGE YET##`

The first line hints to how the art pieces in this challenge have had their names changed from the originals, as figuring out the proper names of the pieces can help decipher at least one clue (as well as give visual confirmation of the solved puzzle). The first is 'letter swap' for 'Loner Meesa', as phonetically after swapping the first letters of the latter, you get 'Mona Lisa'. The second is 'synonym word play' for 'The Seam of Chaos', as the original title is 'The Scream of Nature', and nature and chaos are somewhat synonymous. Finally is 'anagram', as Thy Heartstring is an anagram for 'The Starry Night'.

The second line gives a key clue to solving The Seam of Chaos. Iisha Lemming is a person who worked as a seamstress for Dolly Parton, who famously made the hit song '9 to 5' (hence 'working full time'). This should hint that the 5th and 9th 'chaos' images are the ones that need to be "seamed" together.

The final line hints that a 13th 'chaos' image exists, which is a key clue to finding the duck images necessary for finding Thy Heartstring. Said chaos image by itself actually carries meaningful binary data, unlike the others.

## Hidden Key Clue (Chaos Image #13)

An image that appears similar to the other chaos images made up of random noise, but this one is noticeably incomplete and less random. A small group of coloured pixels exists at the start, which can be read as plain ASCII, giving the messages `This image was unlucky` and `Mallard:4LSB=53600`, the second of which giving a big clue to how to recover the data from the ducky image. The remaining black and white pixels represent binary where white is 1 and black is 0. This binary when converted to ASCII gives the CSS required to reveal [Clue 3](#clue-3).

## Solution(s)

This time there are 3 images to find, the *Loner Meesa Squint*, *The Seam of Chaos* and *Thy Heartstring*. All of these are images of famous paintings with the names changed in ways that range from slightly clever to referencing Jar Jar Binks.

### Loner Meesa Squint (Mona Lisa)

The image was scaled down to 48x48, and saved as a 2-bit bitmap with only black and white colours. The resulting binary file (which uses 1 byte per pixel) is packed into bits where each 1 is a black pixel and each 0 is a white pixel. This is then encoded as a hex string to reduce the length. Through some sort of mathematical anomaly, squinting at this hex (assuming the text is white and the background is black) seems to look like 4 Mona Lisas in a row... possibly because a lot of binary 1s (which occur in black areas) usually results in the `7` and `f` characters in hex which do not take up as much visual space as many other characters. The reason there are 4 are because while the binary takes 8 digits, the hex pairs only take 2, and `8 ÷ 2 = 4`... but enough of me trying to pretend I planned it.

### The Seam of Chaos (The Scream of Nature)

Using a method described on [Wikipedia's Visual Cryptography article](https://en.wikipedia.org/wiki/Visual_cryptography#Example), the image is converted to black and white in the same way as Loner Meesa, but then the image is split into two. Where pixels in the original were black, the split images have complementary pixels of either both black or both white. Where the pixels in the image are white, the split images have differeng pixels - if one is white, the other is black, and vice-versa. Superimposing these split images by XOR-ing the pixels together results in the full image. An extra hint to this is given in the briefing, playing with another meaning to the word 'imposition'. The name of the piece (seaming chaos) also refers to joining the two disordered images.

To complicate things, 10 other similar random noise images are generated, by repeating the process but only saving one of the split images. These images are worthless and in order to get the correct image back, knowledge of which two images actually fit together is required. All the images are given similar filenames `chaos%d.png` where `%d` is the number 1-12. The split images are `chaos5.png` and `chaos9.png`.

These images are all found on the [page](challenge2/thy-heartstring).

### Thy Heartstring (The Starry Night)

The Starry Night is converted to ASCII art using text-image.com, set to randomly use 1s and 0s as characters. These 1s and 0s are copied in plain text (without colour) to a [page](challenge2/thy-heartstring) and coloured black with CSS. A CSS comment in the source makes an apt reference to The Rolling Stones' song "Paint It, Black" to give a clue as to what's going on. The hex of the colours is combined into one long string and converted to base64 to cut down on verbosity.

The base64 'key' is 53,600 characters long. By creating an image (in this case a rubber duck on a blue radial gradient background) with the dimensions 335x320 we can overwrite the least significant 4 bits of the red channel of each pixel with 4 bits of each character, storing one character every 2 pixels. This creates a little bit of noise and loss of quality in the image which is mostly imperceptible to the human eye without zooming in. The most noticable difference is of course in the pure red part of the image (the duck's beak), which will appear a little darker. Additionally, the whiter part of the blue gradient will have a loss of quality.

To recover the art, take the values of every pixel's red component in the duck image, combining two of the red components at a time by doing the bitwise operation `(red1 & 0xF) | ((red2 & 0xF) << 4)` to get each character of the base64. Decode this base64 to hex, and add each grouping of 6 hex chars as CSS colours to the corresponding 1s and 0s on the colourless page.