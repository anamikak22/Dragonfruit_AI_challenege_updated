# Dragonfruit_AI_challenge_updated
Finding that there were a few issues with the previous approach I took, I came up with a different approach for the dye sensor image generation and cancer detection.
### Question 1. Efficient Data Structure to store the images
**Microscopic Image**:
- As the parasite body is known to be a blob like structure of arbitrary shape which wil be a continuous region of black pixels, it will be most efficient to use ***Run-Length-Encoding (RLE)*** for the microscopic image.
- RLE encodes the image as the starting posiion of the pixel and its ending position. However, to ***optimise*** more in terms of space, we only encode black pixels as encoding both white and black pixels would take unnecessary space.
- Each tuple in the RLE list takes 16 bytes (8 for the starting position and 8 for the end position for each row).
- There will be 100,000 elements in the RLE_data and each row can contain upto 10-12 different runs of black pixels.
- The approximate memory that the RLE representation would take will be 100,000\*16\*10 bytes = 15.26 MB
- In the worst case, the black pixels will be present alternatively in each row, hence each row will contain 50,000 elements. Hence, in this case, the occupied memory will be 100,000\*16\*50,000 bytes = 74.5 GB. But this case will be highly unlikely.

**Dye Sensor Image**:
- In this case, the dye will be sparsely located in the image. Therefore, it will be most efficient to store only the coordinates which are lit. Hence, we use a sparse representation to store the images. For that, we chose the list data structure in python that will only store the coordinates from the image that are lit.
- In the worst case, the dye will acquire all the area of the parasite body. Therefore, would require the memory equal to 16*parasite_body_area which can be approx. 149.01 GB (if the parasite body acquires all the area in the image). However, this case is highly impractical.
- Therefore, assuming that the dye acquires 15% of the total parsite body area and the parasite body area is 40% of the total image area, the required memory for the dye sensor image would be approx. 100,000\*100,000\*0.4\*0.15\*16 bytes = 8.94 GB.

### Question 2. Efficient Data Structure to store the images
**Microscopic Image**:
To create the simulated images, we used the idea of a random walk to create more of a blob like structure resembling the real world images of a parasiteitic microorganism.
- Random Walk: The function starts with a seed pixel and performs a random walk to create a more connected structure resembling a blob.
- Filling: It then fills any small gaps within the initial structure to improve the blob-likeness of the parasite shape.
- Noise: We added some small amount of noise for a more realistic appearance.
This approach generates images where the black pixels are more likely to be clustered together, forming a more convincing blob-like parasite shape while still ensuring at least 25% of the area is occupied.
- After generating the image, we encoded the image as RLE data structure.

**Dye Sensor Image**:
- We used sparse representation for dye sensor image. For this, we again used the idea of the random walk so that the generated structure looks like the blood vessels structure (connected).
- Here, we used a certain number of random walks to make the lit pixels look more sparse.
- We then store the coordinates of the lit pixels in a list.

### Question 3. Cancer Detection
**Note**: For a simple scenario, It is assumed that the position of the parasite body in the dye sensor image is same as that in the microscopic image.
- We calculate the total parasite area and total dye area in the same nested loop by iterating for each column for all the runs in every row. Since, we have 100,000 rows and assuming that each row contains black pixel in every column, the time complexity will be O(n<sup>2</sup>).
- We find the fraction of lit area within the parasite body and check whether it exceeds the threshold of 10%. If it does, we return true, false otherwise.

### Question 4. Cancer Detection with optimised runtime
- To optimise the runtine of the cancer detection function, we used the fact that the number of lit pixels will be much lower than the number of parasite pixels. Therefore, to calculate the total_dye amount, we iterate for every lit pixel and check if is present in the parasite body. This calculation can be done in O(number of lit pixels).
- We calculate the parasite area separately in approx, linear time by assuming that each roe contains 10-12 runs of black pixels.
- Therefore, we optimised the above function and reduced the time complxity to linear time.

### Question 5. Alternative Data Structures for Image Compression
- Run-Length Encoding (RLE): (Already implemented) Works well for binary images with large areas of the same value (e.g., parasite background) but might not be the most efficient for complex shapes like the parasite itself. Runtime is generally fast for encoding and decoding.
- LZ77/LZMA: These dictionary-based techniques can achieve better compression than RLE, especially for redundant patterns within the parasite shape. However, they have higher computational complexity, leading to slower runtime compared to RLE.
- Binary Arithmetic Coding: Achieves near-optimal compression but is computationally expensive for both encoding and decoding. Runtime will be slower than RLE and LZMA.
- Quadtrees: Indexing based quad trees can be used for faster information retrieval. However, it will not be as storage efficient as RLE and sparse matrix in our case.

***Actual Runtime:*** Since, for the detection of cancer, we need to generate 100,000*100,000 pixels images which is taking a huge memory while running the generation functions, I could not compute the actual runtime on my computer. However, I computed the actual storage and runtime on a server with sufficient RAM storage. and the results are as follows:

Size of parasite_image as represented by RLE = 5.000099182128906 MB

Size of the dye_image as represented by sparse matrix = 44.937133789063504 MB

The runtime of the cancer detection function is 0.23678351243336995 minutes for 25% parasite area and 0.01% dye area of the total image area

### Question 6. Tools used
- I used Gemini and ChatGPT for writing the code for me to save the time, as time was a constraint in this challenge. However, I modified the codes as per my approach and requirements.
- We can also use LLM techniques for storing the image in the form of vectors using OpenAI or Copilot like we use for the text. ***Note***: This is just an idea that came to my mind.
