# How would I play audio in iOS by Swift

@(Sample notebook)[swift|audio|ios]

## Intro

**Swift** is a brand-new programming language released by Apple. Today I am going to show you how to play audio in iOS by Swift.

## Agenda
- Migrate from Objective-C
- Swift sweet

## Migrate from Objective-C
First I found the code snippet of Objective-C how to play audio:
```objective-c
NSString *path = [[NSBundle mainBundle] pathForResource:@"song" ofType:@"mp3"];
AVAudioPlayer *audio = [[AVAudioPlayer alloc]
initWithContentsOfURL:[NSURL fileURLWithPath:path] error:nil];

[audio play];
```

### Reference
- AV Founddation: [link](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVFoundationFramework/)
- Udacity: [link](https://www.udacity.com/course/viewer#!/c-ud585/l-3331968960/m-3320668749)
