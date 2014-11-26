# How would I play audio in iOS by Swift

@(Sample notebook)[swift|audio|ios]

## Intro

**Swift** is a brand-new programming language released by Apple. Today I am going to show you how to play audio in iOS by Swift.

## Agenda
- Migrate from Objective-C
- Introduce AVAudioEngine in iOS 8

## Migrate from Objective-C
First I found the code snippet of Objective-C how to play audio:
```objective-c
NSString *path = [[NSBundle mainBundle] pathForResource:@"song" ofType:@"mp3"];
AVAudioPlayer *audio = [[AVAudioPlayer alloc]
initWithContentsOfURL:[NSURL fileURLWithPath:path] error:nil];

[audio play];
```
Now how to migrate that into Swift version. Maybe you can follow these steps:

1. Use the target keyword to search from the Swift documentation
2. If found the same one, use it.
3. Otherwise, search for the similiar one from the Swift documentation.


```swift
import AVFoundation
var filePath =  NSBundle.mainBundle().pathForResource("song", ofType: "mp3")
var audioPlayer = AVAudioPlayer(contentsOfURL: filePathUrl, error: nil)
audioPlayer.play()
```
## Introduce AVAudioEngine in iOS 8
TBD

### Reference
- AV Founddation: [link](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVFoundationFramework/)
- Udacity: [link](https://www.udacity.com/course/viewer#!/c-ud585/l-3331968960/m-3320668749)
