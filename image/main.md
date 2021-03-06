# Image
The Image class offers a bunch of image processing features.

Namespace: `\` <br/>
File location: `lib/image.php`

---

## Instatiation

``` php
$img = new Image([ string $file = NULL ], [ bool $flag = FALSE ], [ string $path = '' ]);
```

You can create a new Image object and load an existing image file like this:

``` php
$img = new Image('path/to/your/image.jpg');
```

To create a new empty image, i.e. for creating a captcha, just leave out the first `$file` argument:

``` php
$img = new Image();
```

The constructor also has a 2nd argument, the `$flag` option. Setting it to `TRUE` enables the file history, which can save additional states for the current file.

If your image file is not located in an `UI` directory, you can use the `$path` argument to set its location.

## Processing

### invert
**Invert image**

``` php
$img->invert();
```

### brightness
**Adjust brightness**

``` php
$img->brightness( int $level );
```

`$level` range: -255 to 255

### contrast
**Adjust contrast**

``` php
$img->contrast( int $level );
```

`$level` range: -100 to 100


### grayscale
**Convert to grayscale**

``` php
$img->grayscale();
```


### smooth
**Adjust smoothness**

``` php
$img->smooth( int $level);
```
`$level` range: -8 to 8, but as its been used for a matrix operation, greater values are applyable too, but may lead to unusable results.

### emboss
**Emboss the image**

``` php
$img->emboss();
```

### sepia
**Apply sepia effect**

``` php
$img->sepia();
```

### pixelate
**Pixelate the image**

``` php
$img->pixelate( int $size );
```

`$size` is the block size of a pixel, usually range: 0 - 100

### blur
**Blur the image using Gaussian filter**

``` php
$img->blur( bool $selective );
```

Set `$selective` to `TRUE` to use a selective blur, otherwise a gaussian blur is used.


### sketch
**Apply sketch effect**

``` php
$img->sketch();
```

### hflip
**Flip on horizontal axis**

``` php
$img->hflip();
```

### vflip
**Flip on vertical axis**

``` php
$img->vflip();
```

### crop
**Crop the image**

``` php
$img->crop( int $x1, int $y1, int $x2, int $y2);
```

### resize
**Resize image (Maintain aspect ratio)**

``` php
$img->resize( int $width, int $height, [ bool $crop = TRUE ], [ bool $enlarge = TRUE ]);
```

If `$crop` is `TRUE` the image will be resized to fit with its smallest side into the resize box.
The overflowing margins will be cropped relative to the center, so your resulting image will fully cover your desired width and height values.
If `$crop` is `FALSE` it will be resized to fit with its longest side into the resize box.
If `$enlarge` is `FALSE` the image will not be scaled up to fit in the resize box.

### rotate
**Rotate image**

``` php
$img->rotate( int $angle );
```

### overlay
**Apply an image overlay**

``` php
$img->overlay( Image $img, [ bool|int $align = NULL ]);
```

This is used to merge to images, i.e. for watermarks. You need to provide another Image object and can align that by the bitwise `$align` argument.

Example:

``` php
$img = new \Image;
$img = new \Image('images/south-park.jpg');

$overlay = new \Image('images/watermark.png');
$overlay->resize(100,38)->rotate(90);

$img->overlay( $overlay, \Image::POS_Right | \Image::POS_Middle );
```

Possible values for `$align` can be combined using this options:

x - align:

*   POS_Left
*   POS_Center
*   POS_Right

y - align:

*   POS_Top
*	POS_Middle
*	POS_Bottom

## Rendering
### identicon
**Generate identicon**

``` php
$img->identicon( string $str, [ int $size = 64 ], [ int $blocks = 4 ]);
```

This method renders a unique [identicon](http://en.wikipedia.org/wiki/Identicon) based on the given `$str`.
The `$size` argument defines the width and height of the resulting image. `$blocks` (range 2 - 7) describes the granularity of the pattern blocks inside the identicon.

### captcha
**Generate CAPTCHA image**

``` php
$img->captcha( string $font, [ int $size = 24 ], [ int $len = 5 ], [ string|bool $key = NULL], [ string $path='' ]);
```

This renders a captcha image. Please have a look to this [user guide section about rendering captcha images](plug-ins#captcha-images), to see a little example.
If your font file is not located in an `UI` directory, you can the set its location with the `$path` argument.

## Info
### width
**Return image width**

``` php
$img->width();
```


### height
**Return image height**

``` php
$img->height();
```

### rgb
**Convert RGB hex triad to array**

``` php
$img->rgb( 0xFF0033 ); // returns array( 255, 0, 51 );
```


## Output

### render
**Send image to HTTP client**

``` php
$img->render();
```

You can pass multiple arguments into this method. The first will define the filetype. For instance this example will render the image as PNG file:

``` php
$img->render('png');
```

You can use `png`, `jpeg`, `git` or `wbmp` as first argument. Any additional arguments refers to the appropriate php render method:

*   [imagepng](http://www.php.net/manual/en/function.imagepng.php)
*   [imagejpeg](http://www.php.net/manual/en/function.imagejpeg.php)
*   [imagegif](http://www.php.net/manual/en/function.imagegif.php)
*   [imagewbmp](http://www.php.net/manual/en/function.imagewbmp.php)

### dump
**Return image as a string**

``` php
$img->dump();
```

This method accepts the same arguments like the `render()` method.

## History

The next methods only take effect when the initial `$flag` argument is `TRUE`.

### save
**Save current state**

``` php
$img->save();
```

This will create a new temporary image of the current state.

### restore
**Revert to specified state**

``` php
$img->restore([ int $state = 1 ]);
```

This fetches the original image state from the temp folder.

### undo
**Undo most recently applied filter**

``` php
$img->undo();
```
