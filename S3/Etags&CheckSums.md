## Etags

- Etags are HTTP response headers that represents a resource that has changed or not without requiring to download the object.

- Value of Etags are represented by MD5 hashing function if the object is not encrypted .

- So Etags are useful if you want to check  whether content in the object has changed or not, programatically or using CLI. 

## CheckSums
- A checksum is a calculated value (hash) which is used to verify the integrity of data during  transmission.

- S3 Uses Checksums to verify data integrity of the files.

- If the data is downloaded and manipulated or lost checksum determines it. 

- Algorithms like MD5, SHA-256, or CRC32 generate a checksum from the data in a file
