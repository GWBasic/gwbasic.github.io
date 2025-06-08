---
layout: post
title:  "Objectively comparing audio codecs: The case for lossless"
date:   2016-07-20
categories: mp3 audio
---
I wrote a tool to investigate degradation of audio in lossy compression, like mp3 and aac. With this tool, I demonstrated that high bitrate lossy audio is not sufficient for audiophile-quality recordings; and that a market for lossless recordings is justified.

For the past few years I've been interested in how good, or bad, lossy audio compression really is. When I occasionally listen to lossless versions of my music, I suspect that they sound better; yet the placebo effect means that I don't trust my own ears.

There is evidence that, in practice, a well-trained ear can tell the difference between lossless and lossy audio. When I ripped my DVD-Audio disks, (in surround sound,) I read though [the EBU Evaluations of Multichannel Audio Codecs](https://tech.ebu.ch/docs/tech/tech3324.pdf). The conclusion that I took from the article is that, in the correct conditions, an audiophile will hear the difference between lossy and lossless versions of a recording.

I do find anecdotal evidence that "so and so" ran "such a such" a test and it concluded that no one can tell the difference. I do not trust these anecdotes as audio perception is highly subjective; the test methods must also be scrutinized.

It is very difficult, if not impossible, to find more objective data that compares audio codecs. The kinds of tests performed in the EBU's study require lots of quality equipment, people, and time. A quick and objective way to compare a compressed file with the original is also needed. My program allows for objective comparisons, although the data requires interpretation.

I recently became interested in this topic again, as the [Tidal Music Streaming Service](https://tidal.com/) offers lossless. Most of my music is managed with iTunes match, where it is not practical to use lossless.

It's also important to understand that what's commonly called "lossless" still involves perceptual manipulation in order to save space. Specifically, [noise shaping][3], used to decimate a 24-bit recording master to 16-bit, involves perceptual manipulations. "Its purpose is to increase the apparent signal-to-noise ratio of the resultant signal." As a consequence, a 16-bit lossless flac file, noise shaped from a 24-bit master, will have some high-frequency hiss below the [absolute threshold of hearing](https://en.wikipedia.org/wiki/Absolute_threshold_of_hearing).

The Experiment
==============

To evaluate compression, I decided to rip three different songs from different DVD-Audio disks:

- **Don't Let it Show**, from [I Robot][5], by The Alan Parson's Project. (24-bit, 192 khz.)
- **Don't Go**, from [Magnification][6], by Yes. (24-bit, 96 khz)
- **Flight Test**, from [Yoshimi Battles the Pink Robots][7], by The Flaming Lips. (24-bit, 96 khz.)

I chose two modern recordings to ensure that the full capability and frequency range of 24-bit, high sampling rate, audio, was utilized.

All rips were the stereo version of the songs, and were converted to 48 khz, floating point wav files. I called the 48khz, floating point version my "original." To convert, I used sox:

    sox (24-bit from dvda.wav) -e floating-point original.wav  rate -v 48k

I chose 48khz because lossy codecs, and streaming services, don't support higher sampling rates. This is somewhat of a shame, because "[Under ideal laboratory conditions, humans can hear sound as ... high as 28 kHz][8]." Hopefully sometime I can explore this topic further.

(One curious thing that I learned is that "Don't Let it Show" has no material over 22.05 hz. This really surprised me, as the disk made a very big deal about having a 96 khz version for DVD-Video, and 192 khz for DVD-Audio.)

I created many different compressed versions of each song:

 1. 16-bit noise-shaped flac, via sox
 2. 320 kbps aac, via iTunes
 3. 320 kbps mp3, via xAct Audio Copy
 4. 320 kbps opus, via xAct Audio Copy

I also made some experimental "lossless" decimated versions of Don't Go:

 1. 8-bit noise-shaped flac, via sox
 2. 8-bit noise-shaped flac, with equalization to simulate emphasis and noise reduction, via sox and Audacity
 3. 12-bit noise-shaped flac, via sox

## The Program ##

The source code to my comparison program is available on Github, see [MeasureDegredation][9].

My program expects that both the original and compressed files are 32-bit floating point wav files, 48 khz. This dramatically simplified my implementation, although I did have to rely on external tools, such as sox and Audacity, to convert the compressed wav file to floating point.

I chose floating point because it avoids quantization error and biases that originate from noise shaping. As I demonstrate in this article, decimating to 16-bit can be considered "lossy," depending on how many hairs one wants to split. Thus, it's critical that, for an accurate comparison, lossy files are decoded to floating bit.

The program outputs an excel spreadsheet for further analysis. I tried generating graphs in my program, but I could not. Thus, I had to later manually add the graphs.

## The Data ##

The program performs three different kind of comparisons:

- **Sample-by-Sample**: Effective for examining how directly lossless the compression is
- **Sample-by-Sample in frequency bands**: Allows a closer look at lossless compression by breaking the signal into three frequency bands
- **All Frequencies**: Compares all frequencies to discover equalization changes, and accuracies, at different frequencies.

The Sample-by-Sample comparison is mainly useful for examining how drastically noise-shaping manipulates the original signal; and how close a lossless audio file is to the original. It shows what the average bits per sample is, if compression were merely rounding the source floating-point version, and what the worst bits per sample is.

The Sample-by-Sample comparison is also run again with the signal broken into three frequency components. This allows seeing how noise shaping preserves the original frequency bands in a lossless manner.

The All Frequencies comparison is the most useful, as it shows how either equalization is preserved in a lossless file, or dramatically changed in a lossy file. It also attempts to calculate a "bits per sample" for each of the frequency components that make up a recording. It is based on the theory that the ear only hears frequencies' amplitudes; and not phase. Thus, this is the most objective way that I can evaluate audio compression techniques purely from data.

Someone who understands audio perception better than I do might find fault in my comparison technique. Keep in mind that the goal is to have an objective comparison that does not rely on listening tests. It's always possible to improve the program used to compare, or to re-run tests with improved encoder settings.

What I did not do in the All Frequencies comparison is measure decibels, or decibels of separation. Such an approach would be even closer to measuring how the ear perceives differences in a compressed file.

Ideally, a comparison that results in one number, or a small set of numbers, describing how close the compressed audio file is to the original, is desirable. I did not create this number, though.

Comparing Each Compression Technique
=======

## 16-bit noise-shaped flac ##

**Note:** It is important to remember that decimating from a 24-bit master to 16-bit is a perceptual compression technique. It's considered lossless because it closely matches the fundamental limitations of human hearing. (Although, there is room for a healthy debate.)

All three songs were converted to 16-bit flac with the following command:

    sox original.wav -b 16 16bit.flac dither -s -p 16

After converting, the worst error in all files was approximately 29. This means that, compared to rounding to a 16-bit integer, a noise shaped flac can be approximately ± 29. At its worst, a noise shaped flac is about 11.1 bits per sample. The average error, compared to rounding, is ± 4.7. This gives an average bits per sample of 13.7.

The numbers presented, though, are misleading, as noise shaping is perceptual and is designed to better preserve frequencies where the ear is more sensitive, at the expense of less accuracy where the ear is less sensitive. When looking at different frequency bands:

- **High Frequencies: > 6khz**: Worst bits per sample is 15.25, average is 19.5
- **Mid Frequencies: < 6khz, > 3khz**: Worst bits per sample is about 17, average is 20.1
- **Low Frequencies: < 3khz**: Worst bits per sample is about 16.8, average is 19.5

In all three bands, the equalization was unchanged. **Just by breaking a 16-bit, noise-shaped, quantized signal into bands, it more closely resembles the original.** This is an important point to understand, as the general belief is that no DAC has a signal to noise ratio greater than 21 bit. This means that noise-shaping to 16 bit is very close to the maximum theoretical audio quality achievable on modern equipment.

When looking at each frequency (without looking at phase,) 16-bit noise shaping becomes easier to understand.

Don't Go:
![Don't Go: 16-bit, noise shaped FLAC, bits per sample, per frequency][10]
Don't Go: 16-bit, noise shaped FLAC, bits per sample, per frequency
![Don't Go: 16-bit, noise shaped FLAC, frequency response][11]
Don't Go: 16-bit, noise shaped FLAC, frequency response

----------

Don't Let It Show:
![Don't Let It Show: 16-bit, noise shaped FLAC, bits per sample, per frequency][12]
Don't Let It Show: 16-bit, noise shaped FLAC, bits per sample, per frequency
![Don't Let It Show: 16-bit, noise shaped FLAC, frequency response][13]
Don't Let It Show: 16-bit, noise shaped FLAC, frequency response

----------

Flight Test:
![Flight Test: 16-bit, noise shaped FLAC, bits per sample, per frequency][14]
Flight Test: 16-bit, noise shaped FLAC, bits per sample, per frequency
![Flight Test: 16-bit, noise shaped FLAC, frequency response][15]
Flight Test: 16-bit, noise shaped FLAC, frequency response

----------

All three frequency response graphs have a sparkle in the highest frequencies where the ear is less sensitive. They are essentially flat until about 15khz, and as they get over 20 khz, they are ± 3%. The graph for Don't Let it Show is harder to read, because it shows 0% for frequencies over 22.05 hz. The other graphs are zoomed in at the top percents.

This data makes the term lossless somewhat ambiguous: a 16-bit reduction at 48khz, or 41khz, is generally agreed to be very close to the fundamental limitations of human hearing, but are the changes shown here lossless? Does lossless mean a almost flat frequency response, 18-20 bit resolution where the ear is most sensitive, and 11-13 bit resolution where the ear is less sensitive?

Certainly, the term "mathematically lossless" is well defined, as Flac does not make any changes to an integer (16 bit or 24 bit) audio signal. But, where do we draw the line between lossless and lossy when changes are made, such as when decimating happens when downsampling from 24-bit to 16 bit? How many hairs need to be split to differentiate between lossy and lossless?

In all three cases, a 16-bit lossless Flac was 3x the size of a 320 kbps mp3, aac, or opus file.

## 320 kbps mp3 ##

Mp3 is the oldest perceptual lossy codec that I investigated. Its widespread popularity necessitates investigation; even though it is considered obsolete. In the approximately 20 years that we've used mp3, the encoders improved considerably.

To generate my mp3s, I used xAct Audio copy. I selected ABR at 320 kbps.

When comparing mp3 on a sample - by - sample basis, predictably, the biggest differences were huge. The "worst error" was off by over 40,000 (16-bit,) implying at lossless, a worst-base scenario of less than a bit a sample. On average, mp3 was about 3-4 bits a sample, sounding worse than a cassette tape. Even when comparing at frequency bands, the error was still similar.

Don't Go:
![Don't Go: Bits per sample per frequency][16]
Don't Go: Bits per sample per frequency
![Don't Go: Frequency Response][17]
Don't Go: Frequency Response

----------
Don't Let It Show
![Don't Let It Show: Bits per sample per frequency][18]
Don't Let It Show: Bits per sample per frequency
![Don't Let It Show: Frequency Response][19]
Don't Let It Show: Frequency Response

----------
Flight Test
![Flight Test: Bits per sample per frequency][20]
Flight Test: Bits per sample per frequency
![Flight Test: Frequency Response][21]
Flight Test: Frequency Response

----------

Looking at the data, the low-frequency accuracy is just atrocious at high bitrates. The error is so bad at times that low frequencies have negative bits per sample. The equalization curve is also anything but flat.

Based on this data, I would never use mp3 for any lossy audio, unless hardware limitations forced me to use it. It is completely unacceptable to use for any streaming or pay-to-download service. As a result, I will never pay for anything that uses mp3.

It is time for mp3 to go the way of the 78 shellac record and the 8-track.

## 320 kbps aac ##

Mp3's authors created aac to continue their work and make improvements. It is the natural successor to mp3, and is what iTunes uses.

I encoded all of these files in iTunes. Unlike the other files in this test, these files were encoded a few years ago when I originally bought the DVD-Audio disks. I directly imported the high resolution (and high sampling rate) wav file into iTunes, and relied on iTunes to perform all downsampling to 48khz.

Surprisingly, when looking at sample-by-sample and banded, aac averages over 9 bits per sample for Don't Go. For the other files, the error is much worse.

Don't Go:
![Don't Go: Bits per sample per frequency][22]
Don't Go: Bits per sample per frequency
![Don't Go: frequency response][23]
Don't Go: frequency response

----------

Don't Let it Show:
![Don't Let It Show: Bits per sample per frequency][24]
Don't Let It Show: Bits per sample per frequency
![Don't Let It Show: frequency response][25]
Don't Let It Show: frequency response

----------

Flight Test
![Flight Test: Bits per sample per frequency][26]
Flight Test: Bits per sample per frequency
![Flight Test: Frequency response][27]
Flight Test: Frequency response

----------

For Don't Go and Don't Let it Show, aac had a somewhat flat frequency response, only really showing changes in the ultra-high frequencies that people hear poorly. The bits per sample was low in low frequencies, but steadily climbed.

Surprisingly, Flight Test showed problems. Low frequencies had negative bits per second, and the equalization favored bass too much.

I've happily listened to aac for about a decade, but usually in the car or in office settings. After seeing this data, it's clear that high bitrate aac degrades a file too much for high quality listening.

## 320 kbps opus ##

To encode Opus, I used xAct Audio Copy. I chose VBR constrained with a 60ns window. A short window setting allows for Opus to work in low-latency applications; in this case, I was more concerned about optimizing for audio quality instead of telephony.

One of the most impressive things about opus is that it results in a a file that's, on average, **about equivalent to a rounded-to-8-bit lossless** audio file, both when comparing by samples and when looking at frequency bands. This is very impressive for a "lossy" audio format.

Anecdotally, Opus appeared more CPU intense than the other codecs. I did not measure CPU or compression time in any objective manner, though.

Don't Go:
![Don't Go: Bits per sample per frequency][28]
Don't Go: Bits per sample per frequency
![Don't Go: Frequency Response][29]
Don't Go: Frequency Response

----------

Don't Let It Show:
![Don't Let it Show: Bits per sample per frequency][30]
Don't Let it Show: Bits per sample per frequency
![Don't Let it Show: Frequency response][31]
Don't Let it Show: Frequency response

----------

Flight Test:
![Flight Test: Bits per sample per frequency][32]
Flight Test: Bits per sample per frequency
![Flight Test: Frequency response][33]
Flight Test: Frequency response

----------

When looking at each frequency, opus has a much more impressive bits per sample per frequency. Compared to mp3 and aac, it accurately can represent all frequencies. Frequency response is generally flat, with some dips where the ear is least sensitive.

Objectively, Opus is the most accurate lossy format. When comparing file size, Opus is 2/3rds the size of an 8-bit flac, but without audible hiss or artifacts. Opus's accuracy also emphasizes that the term "lossless" is quite subjective. It has a very flat frequency response, and when comparing on a sample-by-sample basis, comes very close to 8 bits per sample.

## Noise-shaping to lower bits per sample Flac ##

I also decided to experiment slightly with converting Don't Go to different forms of an 8-bit and 12-bit Flac. The 8-bit flac was about half the size of the 16-bit Flac, and only about 1/3rd larger than the 320 kbps lossy file. The 12-bit Flac was double the size of a 320 kbps lossy file.

(I also generated a rounded-to-8-bit file, but it sounded too poorly to be considered.)

Ordinary Noise Shaping to 8-bit
-------

My first experiment was to use ordinary noise shaping:

    sox original.wav -b 8 8bitnoiseshaped.flac dither -s -p 8

This resulted in a hissy sounding file. This hiss was about what a cassette with Dolby B sounds like, although at a higher frequency.

![8-bit, noise shaped, bits per frequency][34]
8-bit, noise shaped, bits per frequency
![8-bit, noise shaped, frequency response][35]
8-bit, noise shaped, frequency response

Some Equalization with noise-shaping at 8-bit
-------

The audio quality, while hissy, was somewhat promising. What if I could apply some analog-style noise reduction? I couldn't find any easy-accessible ways to digitally-emulate a noise reduction circuit, so I equalized the original wave file to have very high treble, and then converted it to an 8-bit Flac. On "playback," I equalized using the inverse.

An interesting side-effect happened. Equalization resulted in lots of clipping; the clipping then resulted in a "pumping" sound in the playback side. I fixed this with normalizing after equalization, both before converting to Flac and after converting from Flac.

Upon applying the inverse equalization, most hiss remained. This could probably be fixed with some compression / expansion, but I will admit that I don't understand these filters well enough to use them for this purpose.

![Equalization in Audacity][36]
Equalization in Audacity

![8-bit bits per frequency with equalization and noise reduction][37]
8-bit bits per frequency with equalization and noise reduction
![8-bit frequency response with equalization and noise reduction][38]
8-bit frequency response with equalization and noise reduction

Looking at the data, noise shaping to 8-bit is promising. I suspect that, when pairing with a digital emulation of analog-style noise reduction, an 8-bits-per-sample approach can yield a pleasant-sounding result.

Noise-shaping to 12-bit
-------

Noise-shaping at 12-bit yielded a very accurate file. On a sample-by-sample basis, it was 9.7 bits per sample. When broken into frequency bands, it was 16.1 bits per sample between 3 and 6khz, and 15.5 bits per sample otherwise.

When I listened to the file, I could not tell any differences. My listening tests, though, are unscientific. I did not listen at a very loud volume, and I suspect that some hiss is present at loud volumes.

When comparing on a frequency-by-frequency basis, equalization was flat until the high frequencies where the ear is less sensitive.

![12-bit, noise shaped, bits per frequency][39]
12-bit, noise shaped, bits per frequency
![12-bit, noise shaped, frequency response][40]
12-bit, noise shaped, frequency response

One thing to point out is that [16-bit audio is generally considered 96 db of separation][41] without noise shaping. This is below the [120db threshold of pain][42]. This means that someone might be able to tell the difference between 12-bit noise shaped and 16-bit noise shaped when listening at loud volumes. I do wonder how 12-bit noise shaped compares to 16-bit rounded. Likewise, could analog-style noise reduction techniques allow for a 120db signal to noise ratio with noise-shaped 12-bit audio?

In Summary
==========

In Summary, whenever audio is presented to discerning listeners, it's critical that a lossless version is available. Appreciating lossless audio is not a placebo effect, but instead is something that is measurable with real data.

Codecs like mp3 and aac are too outdated for modern use. Their equalization changes are too drastic, even for discerning listeners.

Opus is the most promising codec when bandwidth is limited. Its equalization is the flattest of all three lossy codecs, although there are noticeable dips in the graph. When writing a new audio application, using Opus when bandwidth is limited may result in the most pleasing results.

One interesting area to investigate is to see if there is a way to have a slightly smaller file than Flac, but still preserve audio quality. Can older analog-style noise reduction techniques, combined with a noise-shaped 8-bit, or 12-bit, signal, preserve an accurate signal? Is such a technique considered lossy or lossless? For example, can a file that's 2/3rds the size of a 16-bit Flac still be as close to the fundamental limits of human hearing as mathematically-lossless 16-bit audio?

Another point is that the terms lossy and lossless are very subjective. Yes, Flac can preserve a 16-bit, or 24-bit signal without loss, but is decimating 24-bit master to 16-bit lossy? Is 8-bit lossy? Is a lossy codec that results in a signal about as accurate as decimating to 8-bit lossy?

An area of frustration is the amount of software and APIs that I encounter that are locked at 16-bit. Decimating audio to 16-bit requires some perceptual conversions, and these conversions have consequence. This can become apparent when manipulating noise-shaped audio, and then re-noise-shaping it. Modern hardware does not need to be 16-bit; instead, we need to view 16-bit as a compression technique. Software developers should not have to deal with tradeoffs involved in decimating to 16-bit. Audio APIs really need to be 32-bit float; and the audio hardware will need to choose algorithms that convert this to analog at an accuracy unambiguously beyond the human range of hearing.

Thus, the final conclusion is that the term "lossless," (and "cd quality,") must be carefully defined, otherwise, it will be abused as a marketing term. "Lossless" can't become like the term "natural," "farm to fork," or "wholesome," where the retailer can redefine the description to match the product. The way to avoid confusion is to instead require terms like:

- **16-bit, Mathematically lossless**: Basically, Flac from a CD
- **Lossless at 120db**: Completely lossless with a snr of 120db. The signal might be slightly different than mathematically lossless, though.
- **Lossless at 96db**: Completely lossless with a snr of 96db. The signal might be slightly different than mathematically lossless, though.

  [3]: https://en.wikipedia.org/wiki/Noise_shaping
  [5]: https://en.wikipedia.org/wiki/I_Robot_%28album%29
  [6]: https://en.wikipedia.org/wiki/Magnification_%28album%29
  [7]: https://en.wikipedia.org/wiki/Yoshimi_Battles_the_Pink_Robots
  [8]: https://en.wikipedia.org/wiki/Hearing_range#Humans
  [9]: https://github.com/GWBasic/MeasureDegredation
  [10]: https://andrewrondeau.com/blog/content/images/20160707190242-16bit%20-%20bits%20per%20sample%20-don%27t%20go.png
  [11]: https://andrewrondeau.com/blog/content/images/20160707190522-16bit%20-%20frequency%20response%20-%20don%27t%20go.png
  [12]: https://andrewrondeau.com/blog/content/images/20160707194913-16%20bit%20bits%20per%20sample%20-%20Don%27t%20Let%20it%20Show.png
  [13]: https://andrewrondeau.com/blog/content/images/20160707194935-16%20bit%20frequency%20response%20-%20Don%27t%20Let%20it%20Show.png
  [14]: https://andrewrondeau.com/blog/content/images/20160707195016-16%20bit%20bits%20per%20sample%20-%20Flight%20Test.png
  [15]: https://andrewrondeau.com/blog/content/images/20160707195027-16%20bit%20frequency%20response%20-%20Flight%20Test.png
  [16]: https://andrewrondeau.com/blog/content/images/20160711182659-mp3%20-%20bits%20per%20sample.png
  [17]: https://andrewrondeau.com/blog/content/images/20160711182716-mp3%20-%20frequency%20response.png
  [18]: https://andrewrondeau.com/blog/content/images/20160711182830-mp3%20bits%20per%20sample%20-%20Don%27t%20Let%20it%20Show.png
  [19]: https://andrewrondeau.com/blog/content/images/20160711182848-mp3%20frequency%20response%20-%20Don%27t%20Let%20it%20Show.png
  [20]: https://andrewrondeau.com/blog/content/images/20160711182956-mp3%20bits%20per%20sample%20-%20Flight%20Test.png
  [21]: https://andrewrondeau.com/blog/content/images/20160711183010-mp3%20frequency%20response%20-%20Flight%20Test.png
  [22]: https://andrewrondeau.com/blog/content/images/20160711184833-aac%20-%20bits%20per%20sample.png
  [23]: https://andrewrondeau.com/blog/content/images/20160711184851-aac%20-%20frequency%20response.png
  [24]: https://andrewrondeau.com/blog/content/images/20160711184955-aac%20bits%20per%20sample%20-%20Don%27t%20Let%20it%20Show.png
  [25]: https://andrewrondeau.com/blog/content/images/20160711185009-aac%20frequency%20response%20-%20Don%27t%20Let%20it%20Show.png
  [26]: https://andrewrondeau.com/blog/content/images/20160711185116-aac%20bits%20per%20sample%20-%20Flight%20Test.png
  [27]: https://andrewrondeau.com/blog/content/images/20160711185127-aac%20frequency%20response%20-%20Flight%20Test.png
  [28]: https://andrewrondeau.com/blog/content/images/20160711193228-opus%20-%20bits%20per%20sample%20-don%27t%20go.png
  [29]: https://andrewrondeau.com/blog/content/images/20160711193242-opus%20-%20frequency%20response.png
  [30]: https://andrewrondeau.com/blog/content/images/20160711193331-opus%20bits%20per%20sample%20-%20Don%27t%20Let%20it%20Show.png
  [31]: https://andrewrondeau.com/blog/content/images/20160711193346-opus%20frequency%20response%20-%20Don%27t%20Let%20it%20Show.png
  [32]: https://andrewrondeau.com/blog/content/images/20160711193444-opus%20-%20bits%20per%20sample%20-don%27t%20go.png
  [33]: https://andrewrondeau.com/blog/content/images/20160711193502-opus%20-%20frequency%20response.png
  [34]: https://andrewrondeau.com/blog/content/images/20160714174219-8bit%20-%20bits%20per%20sample.png
  [35]: https://andrewrondeau.com/blog/content/images/20160714174245-8bit%20-%20frequency%20response.png
  [36]: https://andrewrondeau.com/blog/content/images/20160718181854-Screen%20Shot%202016-07-15%20at%2011.04.45%20PM.png
  [37]: https://andrewrondeau.com/blog/content/images/20160718181934-8bitemph%20-%20bits%20per%20sample.png
  [38]: https://andrewrondeau.com/blog/content/images/20160718182007-8bitemph%20-%20frequency%20response.png
  [39]: https://andrewrondeau.com/blog/content/images/20160718183227-12bit%20-%20bits%20per%20sample.png
  [40]: https://andrewrondeau.com/blog/content/images/20160718183308-12bit%20-%20frequency%20response.png
  [41]: https://en.wikipedia.org/wiki/Audio_bit_depth
  [42]: https://en.wikipedia.org/wiki/Threshold_of_pain
