# Chewie Fork with SRT subtitle support

Credit to https://github.com/brianegan/chewie for creating Chewie, and to https://github.com/kirill09/chewie for adding subtitle support.  

I've made a few tweaks and added usage instructions below.

Please visit https://pub.dev/packages/chewie for Chewie installation instructions and support.

## Installation

In your `pubspec.yaml` file within your Flutter Project: 

```yaml
dependencies:
  chewie: 
    git: 
      url: https://github.com/LampeMW/chewie
      ref: subtitles
```

## How to use Subtitles

You can use either local SRT files or pull SRT files from a URL

Assets:
```dart
String subtitles = await DefaultAssetBundle.of(context).loadString('assets/SrtFile.srt');
```
URL:
```dart
String subtitles = await NetworkAssetBundle(Uri.parse(<srtUrl>)).loadString(<srtUrl>);
```

Create VideoPlayerController with either a url or local video file
```dart
import 'package:chewie/chewie.dart';
final videoPlayerController = VideoPlayerController.network(
    'https://flutter.github.io/assets-for-api-docs/videos/butterfly.mp4');
```

Create your Chewie Controller:
```dart
final chewieController = ChewieController(
  videoPlayerController: videoPlayerController,
  aspectRatio: 3 / 2,
  autoPlay: true,
  looping: true,
);
```

And set the subtitles:
```dart
chewieController.setSubtitle(subtitles);
```

And finally, create your Chewie widget:
```dart
final playerWidget = Chewie(
  controller: chewieController,
);
```

Please make sure to dispose both controller widgets after use. For example by overriding the dispose method of the a `StatefulWidget`:
```dart
@override
void dispose() {
  videoPlayerController.dispose();
  chewieController.dispose();
  super.dispose();
}
```


## iOS warning

The video player plugin used by chewie is not functional on iOS simulators. An iOS device must be used during development/testing. Please refer to this [issue](https://github.com/flutter/flutter/issues/14647).