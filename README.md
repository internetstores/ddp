# DDP

This is a [DPP](https://github.com/meteor/meteor/blob/devel/packages/ddp/DDP.md) protocol implementation for Flutter/Dart.

## Getting Started
### Install
Add the package to your project using these [instructions](https://pub.dartlang.org/packages/ddp#-installing-tab-).  
For help getting started with Flutter, view our online [documentation](https://flutter.io/).  
For help on editing package code, view the [documentation](https://flutter.io/developing-packages/).

### Connection
```dart
import 'package:ddp/ddp.dart';

DdpClient client = DdpClient("meteor", "ws://localhost:3000/websocket", "meteor");
client.connect();

client.addStatusListener((status) {
  if (status == ConnectStatus.connected) {
    print('I am connected!');
  }
});
```

### Subscribe
```dart
void myListener(String collectionName, String message, String docId, Map map) {
  // Do something great
}

client.addStatusListener((status) {
  if (status == ConnectStatus.connected) {
    client.subscribe("subscribe_name", (done) {
      Collection collection = done.owner.collectionByName("collection_name");
      collection.addUpdateListener(myListener);
    }, []);
  }
});
```

### Call method
```dart
List tasks = [];

client.addStatusListener((status) async {
  if (status == ConnectStatus.connected) {
    var data = await client.call('getTasks', []);
    data.reply.forEach((map) => tasks.add(map));
  }
});
```

## License

Copyright (c) 2018 haoguo

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
