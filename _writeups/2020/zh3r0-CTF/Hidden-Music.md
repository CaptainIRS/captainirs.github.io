---
layout: writeup
category: zh3r0-CTF
chall_description: https://i.imgur.com/MzPupPQ.png
points: 499
solves: 
tags: forensics audio-stego
date: 2020-07-25
comments: true
---

The given file is `this.bmp`

The given hint is just a troll. The challenge has nothing to do with Outguess. However, the actual hint is in the challenge description itself.

![image](https://i.imgur.com/DWiYEzt.png)

So, by using [OpenStego](https://www.openstego.com/) a file named `didYouCheckMyNumbers.gz` can be extracted without a password. However, the extension is just a gimmick and running `strings` on the file reveals some interesting stuff:

![image](https://i.imgur.com/YiHyMiz.png)

We can see that the file is just a concatenation of a bunch of files. The flag in the middle is just a troll. The `MThd` chunk is the first chunk in a MIDI file. Since the challenge title suggests something to do with music, this file is important. Using a hex-editor(I used [hexed.it](https://hexed.it/)), all the bytes preceding `MThd` are deleted and the resulting file is exported as `audio.mid`.

The file is then imported into a sequencer(I used [onlinesequencer.net/](https://onlinesequencer.net/)). Zooming out a bit, the flag is visible:

![image](https://i.imgur.com/YRRtgbV.png)

The flag is `Zh3r0{MUSIC_IS_FUN_DO_TO_DO}`