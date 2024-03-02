# Stegasaurus
Stegasaurus is a fun little LSB-image steganography project that has errors purposely built into it. This project is intended for undergrad students in an introductory software engineering class to practice software testing. I have purposely inserted three errors into the code, although there might be other places where I made mistakes without realizing it! The first two errors would be related to functional requirements--what the software does. The first one is easy enough that all students should discover it. However, the second error is harder to find and will likely require the student to create their own set of test images. The final error is more of a performance issue that would be part of the non-functional requirements. Students who identify this last error will likely need to examine the source code (i.e., whitebox testing).

## Steganography
Steganography is the practice of hiding information. It is related to cryptography, but has a slightly different purpose. In cryptography, the meaning of information is hidden but the presence of the information is known. In steganography, the existence of the information is kept secret, but if somebody knew where to look, they would have no trouble understanding the meaning of the secret data. Of course, these two practices can be combined so that the meaning of information is encrypted and then the resulting ciphertext is hidden with steganography. Both steganography and cryptography have their specific uses, but cryptography is considered the more robust and secure science whereas steganography is more of an art.

## Least Significant Bit Steganography
In least significant bit (LSB) steganography, information is embedded in digital images, audio, or movies by changing the media in a way is undetectable to the end user. For example, somebody might change an audio file by making the sound ever so slightly flatter or sharper but at such a small level that anyone listening to the music would never hear a difference. Or, with a digital picture, somebody might make an individual pixel slightly darker or lighter but the change would be so small as to be inperceptable to an observer. The flatter note and darker pixel would hide a binary zero whereas the sharper note or lighter pixel would hide a binary one. Of course many notes/pixels would need to be changed to hide a significant message, but most pictures contain many pixels and most music contains many notes.

Stegasaurus only supports LSB steganography in digital images. It loads source images with the Python Image Library (PIL) module which means that it can hide information in any image file that can be read with PIL (e.g., images using the JPEG, BMP, PNG, GIF, etc file formats). However, Stegasaurus always saves the final image that contains the secret message in the Portable Network Graphics (PNG) file format because PNG files use a *lossless* compression algorithm.

## Compressed Formats
Many digital media formats are compressed with a *lossy* algorithm. What this means is that the underlying data bytes are compressed in a way that saves lots of storage space but modifies the media in the process. These changes to the underlying data will overwrite the embedded data so that the secret information is permanently lost. In many ways, steganography and lossy compression perform similar operations--they alter media to provide some new functionality but without changing the media in a way that is easily observable. The difference is that the purpose of lossy compression is to save storage space and the purpose of steganogrpahy is to hide information. But the two operations conflict with eachother and whichever one was invoked first will be destroyed by the operation that was invoked second.

## Usage
There aren't any fancy installation instructions using `pip` or `setup.py` because Stegasaurus is a buggy module on purpose. All that you have to do to install it is download the `stega.py` file by saving it to some useful working directory. Stegasaurus includes a command line interface using options that are read by `argparse` or it can be imported as a module into another project.

### How to Use Command Line
Stegasaurus as a program understands two command line parameters: an image file and a message. If you include a message, Stegasaurus will embed the message into the image file and save the result as a PNG. If you omit the message parameter, Stegasaurus will try to extract a "message" from the image but the message will be gibberish if the image never contain an embedded message. It is important to realize that the Stegasaurus command line program can only hide text.

For example, to hide a message:
```
% python stega.py --file flower.png --message "I hope nobody realizes that I'm wearing dirty socks."
%
```

And to extract a message:
```
% python stega.py --file flower.png`
I hope nobody realizes that I'm wearing dirty socks.
%
```
### How to Import the Module
The `stega` module has two public functions: `hide_message` and `extract_message`. Hiding a message requires a PIL image and secret data. The secret information can be text or arbitrary binary data, but it must be provided as a byte string. Extracting a message simply requires a PIL image.

For example, to hide arbitrary data:
```
img = PIL.Image.open(filename)
img = stega.hide_message(img, secret_data)
img.save(filename, "PNG")
```

And to extract arbitrary data:
```
img = PIL.Image.open(filename)
secret_data = extract_message(img)
# and now do something useful with b"secret data"
```
## Contributions
Most of the ideas for this project came from *my* senior project when I was an undergrad. My project was written in C and only worked for bitmaps and I had to process the BMP header to get at the pixel data. This version of Stegasaurus is written in Python and is much shorter and simpler to understand. All of the image format specific header parsing is performed by the PIL library and we get to go straight to the pixel data. However, as I was updating my old project, I took some inspiration from [Devang Jain's Medium article](https://medium.com/swlh/lsb-image-steganography-using-python-2bbbee2c69a2) on reshaping the pixels to a flat array. Although he seems to have gone from a 3D to 2D array whereas I went from 3D all the way to 1D. 
