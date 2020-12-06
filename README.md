# cordova-plugin-crop


> Crop an image with aspect in a Cordova app


## Install

```
$ cordova plugin add https://github.com/7h4r05/cordova-plugin-crop
```


## Usage

```js
plugins.crop(function success () {

}, function fail () {

}, '/path/to/image', options)
```

or, if you are running on an environment that supports Promises
(Crosswalk, Android >= KitKat, iOS >= 8)

```js
plugins.crop.promise('/path/to/image', options)
.then(function success (newPath) {

})
.catch(function fail (err) {

})
```

## API

 * quality: Number

The resulting JPEG quality (ignored on Android). default: 100

 * targetWidth: Number

The resulting JPEG picture width. default: -1

 * targetHeight: Number

The resulting JPEG picture height. default: -1
 
 * aspectWidth: Number

Cropper width aspect. default: 1

 * aspectHeight: Number

 Cropper height aspect. default: 1


## Ionic / Typescript Example Angular 10 Service

<img src="screenshot-example.png" width="250" height="500">

This is an example service that uses ionic-native's built in camera and the 7h4r05@cordova-plugin-crop to create a cropped version of the image and return the file path. 

```js
import { Injectable } from '@angular/core';

import { Camera, CameraOptions} from '@ionic-native/camera/ngx';
import { CropWithAspect } from 'ionic-crop-image-with-aspect';

@Injectable({
    providedIn: 'root'
})
export class CameraService {

    constructor(private camera: Camera,
                private crop: CropWithAspect) {
    }

    takePicture(): Promise<string> {
        return this.getPicture({ sourceType: this.camera.PictureSourceType.CAMERA });
    }

    choosePicture(): Promise<string> {
        return this.getPicture({ sourceType: this.camera.PictureSourceType.SAVEDPHOTOALBUM });
    }

    private getPicture(options: CameraOptions): Promise<string> {
        options = !options ? {} : options;
        options.quality = 80;
        options.destinationType = this.camera.DestinationType.FILE_URI;
        options.encodingType = this.camera.EncodingType.PNG;
        options.mediaType = this.camera.MediaType.PICTURE;
        options.targetHeight = 2000;
        options.targetWidth = 2000;
        options.correctOrientation = true;

        return this.camera
            .getPicture(options)
            .then(async photoUrl => {
                const url = await this.crop.crop(photoUrl, {
                    quality: 80,
                    aspectWidth: 4,
                    aspectHeight: 5
                });
                return url;
            })
            .catch(e => {
                return null;
            });
    }
}

```



### Libraries used

 * iOS: [PEPhotoCropEditor](https://github.com/kishikawakatsumi/PEPhotoCropEditor)
 * Android: [android-crop](https://github.com/jdamcd/android-crop)

## License

MIT © [Jeduan Cornejo](https://github.com/jeduan)
