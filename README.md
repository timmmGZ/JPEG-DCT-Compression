# JPEG-DCT-Compression

```
catalog
│── 1. Preset
│   │──  1.1 Read image, set bits occupied by "Run-Length", use RGB-to-YCbCr or not and block size.
│   └──  1.2 Transfer RGB image to YCbCr Image if you want
│── 2. Transform the image into blocks  
│── 3. DCT (Discrete Cosine Transform)
│   │──  3.1 operate DCT on all the blocks respectively, check the difference between step-2 and step-3  
│   │──  3.2 Why DCT (0,0) or (0) is called DC? and why it is the most light point?
│   └──  3.3 How it looks like transforming the DCT blocks back to image?
│── 4. Quantized DCT(save approximations, omit details)  
│   │── 4.1 Use default quantization table(you can find it in google) to quantize all dct blocks  
│   │── 4.2 Transform the quantized dct blocks back to image, check the difference between step-3.1 and step-4.2  
│   │── 4.3 Try a random bad quantization table to check the difference of result image.  
│   └── 4.4 Decompression(image reconstruction) of 3 kinds of DCT(check difference)  
│── 5. Example of saving one quantized DCT block as small size as it can be  
│   │── 5.1 Zig-Zag process  
│   │── 5.2 Huffman Coding  
│   │── 5.3 Run-Length encoding(with VLI Variable-length-integer encoding)  
│   └── 5.4 Save the result of Run-Length to bytes  
│── 6. Saving as jpeg, do step-5 for all blocks of the image  
│── 7. Decoding  
│   │── 7.1 Run-Length decoding  
│   └── 7.2 Zig-Zag decoding  
│       └── 7.2.1 Another Zig-Zag Decoding  
└── 8. Loading the jpeg, decompression of compressed(encoded) image  
```
# Basic idea of DCT
Using DCT is for extracting the approximation of the photo and omit detail, because human eyes don't perceive changes too much in detail, so .Jpeg uses this idea to filter out the details of the photo for reducing the photo size.  
DCT is similar to Fourier transform, below is doing Fourier transform on the whole picture and then generate its high-pass and low-pass pictures.
![image](https://github.com/timmmGZ/JPEG-DCT-Compression/blob/master/images/fft.png)

# Run all Code cells from beginning to end
## These are the parameters you could modify:
```py
w=8 #modify it if you want, maximal 8 due to default quantization table is 8*8
h=w
runBits=1 #modify it if you want
bitBits=3  #modify it if you want
useYCbCr=True #modify it if you want
useHuffman=True #modify it if you want
quantizationRatio=1 #quantization table=default quantization table * quantizationRatio
```
## For above default parameters, the result is below
I only focus on explanations, so didn't improve duration that much:  

Image original size:    2.250 MB  
Compression image size: 0.110 MB  
Compression ratio:      20.41 : 1  
Encoding: 12.198627233505249 seconds  
Decoding: 2.6317562580108643 seconds  
Original image:
![image](https://github.com/timmmGZ/JPEG-DCT-Compression/blob/master/images/coffee.png)
Compression image:
![image](https://github.com/timmmGZ/JPEG-DCT-Compression/blob/master/images/coffeeCompression.png)
