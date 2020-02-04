---
layout: post
title: How are p@55w0rd5 stored?
published: true
image: /img/password-hashing/pwd.jpg
tags: [password,programming]
subtitle: using salt and pepper!
---
Did you ever wonder why you have to reset your password when you click on *Forgot Password*? Why can't the website just send you back your old password to your registered email?

![1](https://media.giphy.com/media/26BROFLJSFhP0cMGk/giphy.gif){: .center-block :}

Because they **DON'T KNOW YOUR PLAIN TEXT PASSWORD!**, they only store the **HASH** of your plain text password!
<!-- If someone cracks a website's database he will get all the passwords but **hashed**. Since hashing function is **not reversible** he won't make anything with it. -->

### What is password hashing?

Hashing ***takes a block of data and returns a string such that you can't get your original block of data back.*** Hash algorithms are one way functions. They turn any amount of data into a fixed-length *"fingerprint"* that **cannot** be reversed. They also have the property that if the input changes by even a tiny bit, the resulting hash is completely different

![2](https://media.giphy.com/media/IgLIVXrBcID9cExa6r/giphy.gif){: .center-block :}

Commonly used hashing algorithms include Message Digest (MDx) algorithms, such as MD5, and Secure Hash Algorithms (SHA), such as SHA-1 and the SHA-2 family that includes the widely used SHA-256 algorithm

### But, how's the user authenticated?

The process of authentication in a hash-based account system is as follows:
1. The user creates an account.
2. Their password is hashed and the **hash** is stored in the database.
3. When the user logins, the hash of the password they entered is checked against the hash of their real password (retrieved from the database).
4. If the hashes match, the user is granted access. If not, the user is told they entered invalid login credentials.

Steps 3 and 4 repeat every time someone tries to login to their account.

In step 4, never tell the user if it was the username or password they got wrong. Always display a generic message like *"Invalid username or password."* This prevents attackers from enumerating valid usernames without knowing their passwords.

### Salting passwords!

![3](https://media.giphy.com/media/l4Jz3a8jO92crUlWM/200w_d.gif){: .center-block :}

Salting hashes sounds like one of the steps of a cooking recipe, but in cryptography, the expression refers to adding random data to the input of a hash function to guarantee a unique output, the hash, even when the inputs are the same. Consequently, the unique hash produced by adding the salt can protect against different attack vectors, such as rainbow table attacks, while slowing down dictionary and brute-force attacks.

Below is a sample to create and validate a hash using the following NuGet package
*Microsoft.AspNetCore.Cryptography.KeyDerivation*

``` c#
public class Hash  
 {  
     public static string Create(string value, string salt)  
     {  
         var valueBytes = KeyDerivation.Pbkdf2(  
                             password: value,  
                             salt: Encoding.UTF8.GetBytes(salt),  
                             prf: KeyDerivationPrf.HMACSHA512,  
                             iterationCount: 10000,  
                             numBytesRequested: 256 / 8);  
  
         return Convert.ToBase64String(valueBytes);  
     }  
  
     public static bool Validate(string value, string salt, string hash)  
         => Create(value, salt) == hash;  
 } 
```

### Conclusion

We must guard user accounts from both internal and external unauthorized access. Cleartext storage must never be an option for passwords. Hashing and salting should always be part of a password management strategy. Salts create unique passwords even in the instance of two users choosing the same passwords. Salts help us mitigate rainbow table attacks by forcing attackers to re-compute them using the salts.

Below are some hashing libraries:

| Platform | CSPRNG |
| :--------- |:--- |
| PHP | [mcrypt_create_iv](https://www.php.net/manual/en/function.mcrypt-create-iv.php), [openssl_random_pseudo_bytes](https://www.php.net/manual/en/function.openssl-random-pseudo-bytes.php) |
| Java | [java.security.SecureRandom](https://docs.oracle.com/javase/6/docs/api/java/security/SecureRandom.html) |
| Dot NET (C#, VB) | [System.Security.Cryptography.RNGCryptoServiceProvider](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rngcryptoserviceprovider?redirectedfrom=MSDN&view=netframework-4.8) |
| Ruby | [SecureRandom](https://rubydoc.info/stdlib/securerandom/1.9.3/SecureRandom) |
| Python | [os.urandom](https://docs.python.org/3/library/os.html) |


