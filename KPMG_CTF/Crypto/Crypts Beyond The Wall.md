

Winter is coming!

[http://kicyber2024-1dfa2c9b2c93-maytheoldgodshelpyou-1.chals.io](http://kicyber2024-1dfa2c9b2c93-maytheoldgodshelpyou-1.chals.io/)


I went to the page and looked into web source

![[Pasted image 20240810184847.png]]

```html
|<!DOCTYPE html>|
||<html lang="en">|
||<head>|
||<meta charset="UTF-8">|
||<meta name="viewport" content="width=device-width, initial-scale=1.0">|
||<title>Beyond the Wall Chat</title>|
||<!-- Old Nan used to tell little Brandon "In the world of cybersec, a well-encrypted VG9ybXVuZC50eHQ= is as secure as secrets whispered beyond the Wall."-->|
||<style>|
||body {|
||background-color: #36454f;|
||color: #ffffff;|
||font-family: Arial, sans-serif;|
||margin: 0;|
||padding: 0;|
||display: flex;|
||flex-direction: column;|
||align-items: center;|
||justify-content: center;|
||height: 100vh;|
||}|
|||
||h1 {|
||font-size: 2rem;|
||margin-bottom: 20px;|
||}|
|||
||.conversation {|
||background-color: #1e272e;|
||padding: 20px;|
||border-radius: 10px;|
||box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);|
||animation: fadeIn 1s;|
||}|
|||
||p {|
||margin: 0;|
||padding: 0;|
||line-height: 1.5;|
||}|
|||
||strong {|
||color: #ffdd55;|
||}|
|||
||/* Animation */|
||@keyframes fadeIn {|
||from {|
||opacity: 0;|
||}|
||to {|
||opacity: 1;|
||}|
||}|
|||
||.watermark {|
||position: fixed;|
||bottom: 2vw;|
||right: 2vw;|
||width: 5vw;|
||height: auto;|
||opacity: 0.5;|
||pointer-events: none;|
||}|
||</style>|
||</head>|
||<body>|
||<h1>Somewhere Beyond the Wall</h1>|
||<div class="conversation">|
||<p><strong>Jon Snow:</strong> Three Eyed Raven, as we venture beyond the Wall, the wildlings' messages grow more cryptic. I fear we may need more than just swords to decipher their intentions.</p>|
||<p><strong>Three Eyed Raven:</strong> Jon Snow, beyond the Wall, secrets are as plentiful as the snowflakes. Consider this like discovering the hidden truths of the White Walkers.</p>|
||<p><strong>Jon Snow:</strong> The White Walkers? But how does that relate to unraveling these messages?</p>|
||<p><strong>Three Eyed Raven:</strong> Much like their icy magic conceals their true purpose, these messages are veiled in secrecy. Think of it as a puzzle, where a certain <em>key</em> is needed to unlock the message's power.</p>|
||<p><strong>Jon Snow:</strong> A cipher? What's the <em>key</em> to unlocking these secrets?</p>|
||<p><strong>Three Eyed Raven:</strong> Just as obsidian can shatter a White Walker, the <em>key</em>, Jon, is like the warmth of a hearth fire on a freezing night. Sometimes, it's as simple and comforting as a glass of warm mead or your friend's favourite drink.</p>|
||<p><strong>Jon Snow:</strong> Ah, I see what you did there, Three Eyed Raven. So, finding the right <em>key</em>, like a comforting glass of milk, is the path to revealing these hidden meanings?</p>|
||<!--<p><strong>Three Eyed Raven:</strong> Indeed, Jon. Like the destiny that lies beyond the Wall, the <b>key</b> will reveal itself when the time is right, much like a Vigenere unveiling its secrets.</p>-->|
||</div>|
|||
||<img src="[ldrago.png](https://kicyber2024-1dfa2c9b2c93-maytheoldgodshelpyou-1.chals.io/ldrago.png)" alt="Watermark" class="watermark">|
||</body>|
||</html>|
||
```

In the header I found base-64 encoded string

`VG9ybXVuZC50eHQ=`

decrypting it I got  `Tormund.txt`

![[Pasted image 20240810185100.png]]

`For Tormund Giantsbane, the wild warrior from beyond the Wall, every conquest is an epic tale. Just like his quest for the BeyondTheWallLogs.txt, where he searches for secrets as mighty as giants. And when it comes to his beloved giants, the key to their loyalty and strength is as simple as it is crucial, giantsmilk. It's a bond as unbreakable as the ice wall, a testament to the power of nature and camaraderie.`

Now I found another txt file 
``
`BeyondTheWallLogs.txt`

In this web page I got

![[Pasted image 20240810185308.png]]

`Ov tux jqiww un cerhfwrbgxhl, pzqzp gnqscxje wq ckkrrmk qksy zprbnyt bso iwrebvazd yl koqx, ldcp zueee eaqa tx zpe ftudmo /C3iz3Tf0ylT3ElvR4vD83LhfP.pewr. Rufm se lckmwnf zmmzo dnmie agmzooj brrtkgzpc, kvcerhfqzx yifrzmmzoc zpe zrkfmcska os wsfi, pxycrvgy avwi zpe jhjfpj wgg uaegos tdy mnvzembtm kubetuq.`

If you paid attention and read the footer of the first web page you will find it mentioned a vigenere
cipher

Decrypting it we get

`In the realm of cryptography, where whispers of secrets echo through the corridors of code, true power lies in the sacred /S3cr3Ts0ftH3WalL4nD83YonD.html. Just as dragons guard their hoarded treasures, encryption safeguards the mysteries of data, ensuring only the worthy may unlock its enigmatic embrace.`

you are ordered to move to another web page `/S3cr3Ts0ftH3WalL4nD83YonD.html`

Navigating to that webpage
![[Pasted image 20240810185812.png]]

The flag is `KPMG_CTF{2neG9iYnCACzqmrMKBIUxoaRsH95yTnaVspcx_mF9w7WuQg0Nj1biN35oeLCe6EwTO-Ysav3UDEdXdTvY2WhxRQyhxCNxn7I-iNAxnEp}`

