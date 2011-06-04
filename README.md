erl_img: Erlang image processing library
========================================

erl_img is a library for reading and writing image files. The goal is to
provide the Erlang community with functionality similar to ImageMagick. erl_img
was originally excavated from the Jungerl repository, but a number of
modifications have since been made.

API
---

To use erl_img, you will need to include the erl_image.hrl header file to get
the appropriate record definitions. The erl_img API is:

    % Read
    erl_img:load("/path/to/image.ext") -> {ok, IMG#erl_image}

    % Get the MIME type
    erl_img:mime_type(IMG) -> "image/something"

    % convert to PNG
    IMG1 = IMG#erl_image{ type = image_png, filename = "/path/to/image.png" }

    % Write
    erl_img:save(IMG1) -> ok


Formats
-------

As of this writing, the number of image formats is fairly limited. The following
table summarizes file format support present in erl_img:

    Format      Read/Write/Metadata?
    
    BMP         Read only
    GIF         Read only
    JPEG        Metadata only
    PNG         Read/Write/Metadata
    TGA         Read/Write
    TIFF        Read/Metadata

Even if your desired format can be written, note that you may have to perform
addtional processing to convert one format to another. Pixel information is
stored in a number of formats internally, and some file types only support some
formats. For example, if you read a file in BGR format, but the destination
file type only supports RGB, it is your responsibility to perform the
pixel-by-pixel channel reversal.

The following table summarizes the pixel formats that can be read and written
by each file type.

                  BMP  GIF  PNG  TGA  TIFF

    b8g8r8        R
    gray*                   RW        R
    r8g8b8                  RW   RW   R
    r8g8b8a8                RW   RW   R
    r16g16b16               RW
    r16g16b16a16            RW
    palette*           R    RW        R