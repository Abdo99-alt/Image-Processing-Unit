![image](https://github.com/user-attachments/assets/87954709-facf-43ab-86f0-542be55356de)

# Image Processing IP 

Assume the input image is 512 * 512 pixel in grey scale, so each pixel can be represented by 8 bits.
## Control Logic:
- 4 line buffers are used to store read pixels from the image. If you wonder why 4 line buffers are used, well the answer is this will accelerate the operation so when the convolution unit is processing on 3 line buffers’ pixels the fourth line buffer is being filled with new pixels. 
- The multiplexers’ select signal depends on the number of pixels that have been read from the line buffer (When it reaches 512).
- Furthermore, additional MUXes before the line buffers are a must to determine to which line buffer the new pixel data will be written.
## Writing to line buffers:
- To determine to which line buffer the new pixel data will be written, pixelCounter is used to count the read pixels. Once it reaches 512 (since the width of the input image is 512) we assert the write enable (i.e., i_data_valid) signal of subsequent line buffer and de-assert that of the already full line buffer.
- Note that i_pixel_data_valid has to be set to enable writing to the line buffers. It’s an AXI control signal.
## Reading from line buffers:
- We start processing the pixels from line buffers only when 3 line buffers have been filled so when totalPixelCounter reaches 1536 the rd_line_buffer signal is asserted.  
- rdCounter needs to incremented by 1 every cycle we read from the line buffers since it will be used to determine from which line buffer new pixels should be read. In other words, when it reaches 512, the read enable signal (i_rd_data) of the fourth line buffer will be asserted and deassert the read enable signal of the appropriate line buffer according to the scheme mentioned in the videos.
- Each 3 pixels read from every line buffer are concatenated together to form the new 9 pixels to be processed in an output named o_pixel_data.
  
![image](https://github.com/user-attachments/assets/1fd3f087-7a2f-403b-9d45-748ec5670c15)
