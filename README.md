# Dragonfruit_AI_challenge_updated
### Question 1. Efficient Data Structure to store the images
**Microscopic Image**:
- As the parasite body is known to be a blob like structure of arbitrary shape which wil be a continuous region of black pixels, it will be most efficient to use ***Run-Length-Encoding (RLE)*** for the microscopic image.
- RLE encodes the image as the starting posiion of the pixel and its run-length. However, to ***optimise*** more in terms of space, we only encode black pixels as encoding both white and black pixels would take unnecessary space.
- Each tuple in the RLE list takes 8 bytes (4 for the starting position and 4 for the run length).
- Assuming the blob to be of circular shape, and occupying an area of 25% in the image which means that the radius of that circle will be approx. 15,000 pixels. Therefore, the number of elements in the RLE list would be approx. 30,000 (1 element per row). The total memory occupied will approx. be 240,000 bytes ≈ 234.4 KB.
- In the worst case, is the parasite occupies each complete image area, there will be 100,000 elements in the RLE data. Hence, in this case, the occupied memory will be 781.25 KB. This proves the storage efficiency of the chosen data structure.

**Dye Sensor Image**:
- In this case, the dye will be sparsely located in the image. Therefore, it will be most efficient to store only the coordinates which are lit. Hence, we use a sparse representation to store the images. For that, we chose the list data structure in python that will only store the coordinates from the image that are lit.
- In the worst case, the dye will acquire all the area of the parasite body. Therefore, would require the memory equal to 4*parasite_body_area which can be approx. 37.26 GB (if the parasite body acquires all the area in the image).
- However, this case is very unrealistic. Therefore, assuming that the dye acquires 30% of the total parsite body area and the parasite body area is 50% of the total image area, the required memory for the dye sensor image would be approx. 1 GB.

### Question 2. Efficient Data Structure to store the images
**Microscopic Image**:
To create the simulated images, we used the idea of a random walk to create ore of a blob like structure resembling the real world images of a parasiteitic microorganism.
- Random Walk: The function starts with a seed pixel and performs a random walk to create a more connected structure resembling a blob.
- Filling: It then fills any small gaps within the initial structure to improve the blob-likeness of the parasite shape.
- Noise: We added some small amount of noise for a more realistic appearance.
This approach generates images where the black pixels are more likely to be clustered together, forming a more convincing blob-like parasite shape while still ensuring at least 25% of the area is occupied.
- After generating the image, we encoded the image as RLE data structure.

**Dye Sensor Image**:
- We used sparse representation for dye sensor image. For this, we randomly set a pixel value as 1 (lit) while ensuring the sparsity of the lit pixels.
- We then store the coordinates of lit pixels in a list.

### Question 3 & 4. Cancer Detection with optimised runtime
**Note**: For a simple scenario, It is assumed that the position of the parasite body in the dye sensor image is same as that in the microscopic image.
- We first decode the microscope image from the RLE data to check that whether the lit pixel is within the parasite body or outside.
- We find the fraction of lit area within the parasite body and check whether it exceeds the threshold of 10%. If it does, we return true, false otherwise.
- For optimising the runtime, we might vectorise the things which would eliminate the for loops and therefore, reduce the runtime.

### Question 5. Alternative Data Structures for Image Compression
- Run-Length Encoding (RLE): (Already implemented) Works well for binary images with large areas of the same value (e.g., parasite background) but might not be the most efficient for complex shapes like the parasite itself. Runtime is generally fast for encoding and decoding.
- LZ77/LZMA: These dictionary-based techniques can achieve better compression than RLE, especially for redundant patterns within the parasite shape. However, they have higher computational complexity, leading to slower runtime compared to RLE.
- Binary Arithmetic Coding: Achieves near-optimal compression but is computationally expensive for both encoding and decoding. Runtime will be slower than RLE and LZMA.
- Quadtrees: Indexing based quad trees can be used for faster information retrieval. However, it will not be as storage efficient as RLE and sparse matrix in our case.

### Question 6. Tools used
- I used Gemini and ChatGPT for writing the code for me to save the time, as time was a constraint in this challenge. However, I modified the codes as per my approach and requirements.
