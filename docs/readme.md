# Docs

## How to determine required features?

~~~
aapt dump badging file.apk
~~~

## How to get Android JA3?

First install:

https://github.com/emanuele-f/PCAPdroid

Start app, then change from HTTP Server to PCAP File. Then click start, if
prompted to save, choose Downloads. Start Android Chrome and wait for a page to
load. Then stop monitoring, and copy file to computer:

~~~
adb ls /sdcard/Download
adb pull /sdcard/Download/PCAPdroid_22_Oct_15_19_28.pcap
~~~

Then my other package can get you the rest of the way:

https://godocs.io/github.com/89z/parse/crypto

## How to get Android public key?

Use a program like this:

~~~go
package main

import (
   "fmt"
   "github.com/89z/parse/protobuf"
   "net/http"
)

func main() {
   src := protobuf.Message{
      {3, ""}: "", {4, ""}: "",
   }
   req, err := http.NewRequest(
      "POST", "http://android.clients.google.com/checkin", src.Encode(),
   )
   if err != nil {
      panic(err)
   }
   req.Header.Set("Content-Type", "application/x-protobuffer")
   res, err := new(http.Transport).RoundTrip(req)
   if err != nil {
      panic(err)
   }
   defer res.Body.Close()
   dst, err := protobuf.Decode(res.Body)
   if err != nil {
      panic(err)
   }
   fmt.Println(dst)
}
~~~

Check the messages under key 5 until you find a key matching:

~~~
google_login_public_key
~~~

The value should look something like this:

~~~
AAAAgMom/1a/v0lblO2Ubrt60J2gcuXSljGFQXgcyZWveWLEwo6prwgi3iJIZdodyhKZQrNWp5nKJ3sr
RXcUW+F1BD3baEVGcmEgqaLZUNBjm057pKRI16kB0YppeGx5qIQ5QjKzsR8ETQbKLNWgRY0QRNVz34kM
JR3P/LgHax/6rmf5AAAAAwEAAQ==
~~~

## How to get Protocol Buffer fields?

Check `com.android.vending` with AndroGuard, with the last working version
(2016):

~~~
versionCode='80441400' versionName='6.1.14'
~~~

## How to get TV apps?

https://play.google.com/store/apps/details?id=com.iqiyi.i18n.tv

In general, you can probably just get the regular app instead:

https://play.google.com/store/apps/details?id=com.iqiyi.i18n

## How to install split APK?

~~~
adb install-multiple (Get-ChildItem *.apk)
~~~
