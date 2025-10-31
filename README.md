##  đầu tiên ta tạo private RSA ở đây mình sử dụng openssl để tiến hành thêm

<img width="1067" height="326" alt="image" src="https://github.com/user-attachments/assets/0cca1cca-3b44-49f9-8914-4bf51b85200d" />
<img width="814" height="121" alt="image" src="https://github.com/user-attachments/assets/217030d9-6ed3-4d2f-9a4a-357d661800f5" />

 ##  Tạo Signature field
 
 <img width="509" height="326" alt="image" src="https://github.com/user-attachments/assets/888bfee7-f44f-4608-9128-f97025230a9e" />
 
 ##  xác định Tính hash (SHA-256/512) trên vùng ByteRange
 
 <img width="830" height="306" alt="image" src="https://github.com/user-attachments/assets/e4abbcb2-f298-432f-acf3-37db1685b385" />
 
 ##  Tạo PKCS#7/CMS detached hoặc CAdES:- Include messageDigest, signingTime, contentType.- Include certificate chain.
 # Tạo file PKCS#7
 ![Uploading image.png…]()
![Uploading image.png…]()

