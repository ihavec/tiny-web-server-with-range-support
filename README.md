Tiny Web server so that it cansupport http requests that asks for a range of a resource rather than the whole resource.

Supporting range request can make network communication more efficient when transferring large files. The browser can resume getting part of a large file if previous transmission onlypartially succeeded. For large media files, range request must be supported to make it possibleto select random position to play. In particular, Safari web browser will not play any large mediafile at all if range request is not supported.

Your task for this assignment is to produce the right response based on the value of thisrangeNode struct's value. If there was no valid range request, your tiny web server should returnthe same response and file content as before, but if there is a valid range request, then your tinyweb server's response has to accommodate the range requests. This includes the following changes you will have to make to your response.Understand ranges:

There are three type of ranges a browser can request:
1. bytes=r1-r2   (r1, r2 are both non negative, they are the byte position of first bytes andlast bytes we retrieve from the  file. A file's byte position start from 0 and last byte is atposition filesize-1.)
2. bytes=r1-    (similar to first case but missing r2, server needs to assume r2 is the lastbyte of the file.)
3. bytes=-r1 (the last r1 bytes of the file, so for a file of size 100, -10 means range 90-99)

Response status:
● Non range request has response status line as below:
  ○ HTTP/1.1 200 OK

● A valid range request should have the status line as this:
  ○ HTTP/1.1 206 Partial Content
  
● An invalid range request, where the range doesn't include any valid byte of therequested file, should have the status line    as this:
  ○ HTTP/1.1 416 Range Not Satisfiable
  
Response headers for range request:
● Content-Range
  ○ example of valid range request: ranges, file size are included in this header.
     ■ Content-Range: bytes 111-120/121
  ○ example of invalid range request: *(for ranges) and file size are included in thisheader.
     ■ Content-Range: bytes */121
● Content-length (for valid ranges, the range is inclusive, so for 111-120 is 10 bytes)
  ○ Content-length: 10
● Accept-Ranges: bytes
  ○ should always be included to acknowledge the browser that we support rangerequest with units being bytes
  
The file content that we sent back should only be the bytes corresponding to the valid range, not the whole file. 
