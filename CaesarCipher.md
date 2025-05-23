# Caeser Cipher
Caeser Cipher using with different key values

### Submitted By : Prajin S
### Register Number : 212223230151

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithm. 

### Step 2:

Implementation using C code.

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
void caesarEncrypt(char *text, int key) {
    for (int i = 0; text[i] != '\0'; i++) { 
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
        }
        else if (c >= 'a' && c <= 'z') {
            text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
        }
    }
}
void caesarDecrypt(char *text, int key) {
    caesarEncrypt(text, -key);  // Decryption is just encryption with a negative key
}
int main() {
    char message[100]; 
    int key;
    printf("Enter the message to encrypt: ");
    fgets(message, sizeof(message), stdin);
    size_t len = strlen(message);
    if (len > 0 && message[len - 1] == '\n') {
        message[len - 1] = '\0';
    }
    printf("Enter the key: ");
    scanf("%d", &key); 
    caesarEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);
    caesarDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);
    return 0;
}

```


## OUTPUT:
TEXT TO BE ENCRYPTED : caesar cipher by Prajin

KEY VALUE : 7

ENCRYPTED TEXT : jhlzhy jpwoly if Wyhqpu

DECRYPTED TEXT : caesar cipher by Prajin
 
![Screenshot 2025-03-15 143759](https://github.com/user-attachments/assets/1d869ba6-3ff6-4d8f-90cc-556379b1266f)

![Screenshot 2025-03-15 143824](https://github.com/user-attachments/assets/3c7428db-aec5-4ce9-bf18-eed48c7923cf)


## RESULT:
The program is executed successfully and results are verified as well.
