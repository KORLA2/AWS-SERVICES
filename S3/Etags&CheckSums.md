## Etags

- Etags are HTTP response headers that represents a resource that has changed or not without requiring to download the object.

- Value of Etags are represented by MD5 hashing function if the object is not encrypted .

- So Etags are useful if you want to check  whether content in the object has changed or not, programatically or using CLI. 

## CheckSums
- A checksum is a calculated value (hash) which is used to verify the integrity of data during  transmission.

- S3 Uses Checksums to verify data integrity of the files.

- If the data is downloaded and manipulated or lost checksum determines it. 

- Algorithms like MD5, SHA-256, or CRC32 generate a checksum from the data in a file

Etags are for our reference and Checksums are for AWS reference whether or not the data in the Object has changed or not.

### How to get the Etags for a Object in the Bucket.

```
![image](https://github.com/user-attachments/assets/16bc8c38-1c8b-480e-9551-574c361414db)


```



