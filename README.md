MIME type
==

Library to get MIME type from a file name or a file path and get the file extension from a MIME type.
When a HTTP server sends a file to the client, MIME type of the file must be set to the Content-Type header of the response.

Only three methods are available:

+   String `mime(String FileName)` //
    gets the MIME type from a file name (such as 'Hello.dart') or a file path (such as '..\Hello.dart').
 
+   String `mimeFromExtension(String extension)` //
    gets the MIME type from an extension (such as 'dart')
    
+   String `extensionFromMime(String mime)` //
    gets extension from MIME type (returns null if there is no such mime type)

All methods return null if they cannot find a match.

## Example


```dart
import 'package:mime_type/mime_type.dart';

sendFile(HttpRequest request, String fileName) {
  HttpResponse response = request.response;
  File file = new File(fileName);
  if (file.existsSync()) {
    String mimeType = mime(fileName);
    if (mimeType == null) mimeType = 'text/plain; charset=UTF-8';
      // you can change the default content type
      // or, you can choose to send error message
    response.headers.set('Content-Type', mimeType);
    RandomAccessFile openedFile = file.openSync();
    response.contentLength = openedFile.lengthSync();
    openedFile.closeSync();
    file.openRead().pipe(response);
  } else {
    // send 404 (Not Found) status to the client
  }
}
```

## License
This library is licensed under [MIT License](http://www.opensource.org/licenses/mit-license.php).
