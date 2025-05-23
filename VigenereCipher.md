# Vigenere Cipher
Vigenere Cipher using with different key values

### Submitted By : Prajin S
### Register Number : 212223230151

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithm. 

### Step 2:

Implementation using C code.

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:

The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
```C
#include <stdio.h>
#include <string.h>
#include <ctype.h>
void sanitizeKey(char key[]) {
    int j = 0;
    for (int i = 0; key[i] != '\0'; i++) {
        if (isalpha(key[i])) {
            key[j++] = key[i]; 
        }
    }
    key[j] = '\0'; 
}
void vigenereEncrypt(char text[], const char key[]) {
    int textLen = strlen(text);
    int keyLen = strlen(key);
    int keyIndex = 0;
    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (isalpha(c)) {  
            char keyChar = toupper(key[keyIndex % keyLen]); 
            if (isupper(c)) {
                text[i] = ((c - 'A' + (keyChar - 'A')) % 26) + 'A';
            } else { 
                text[i] = ((c - 'a' + (keyChar - 'A')) % 26) + 'a';
            }
            keyIndex++;  
        }
    }
}
void vigenereDecrypt(char text[], const char key[]) {
    int textLen = strlen(text);
    int keyLen = strlen(key);
    int keyIndex = 0;
    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (isalpha(c)) {  
            char keyChar = toupper(key[keyIndex % keyLen]); 
            if (isupper(c)) {
                text[i] = ((c - 'A' - (keyChar - 'A') + 26) % 26) + 'A';
            } else { 
                text[i] = ((c - 'a' - (keyChar - 'A') + 26) % 26) + 'a';
            }
            keyIndex++;  
        }
    }
}
int main() {
    char key[100], message[1000];
    printf("Enter encryption key (only letters): ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = 0;
    sanitizeKey(key);
    if (strlen(key) == 0) {
        printf("Error: Key cannot be empty after removing spaces!\n");
        return 1;
    }
    printf("Enter message to encrypt: ");
    fgets(message, sizeof(message), stdin);
    message[strcspn(message, "\n")] = 0; 
    printf("\nOriginal Message: %s\n", message);
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);
    return 0;
}
```

## OUTPUT:

Encryption key : vignerecipher

Message to encrypt: i am safe Prajin

Encrypted Message: d is fewi Rzpqme

Decrypted Message: i am safe Prajin

![Screenshot 2025-03-17 162433](https://github.com/user-attachments/assets/5a9fd7d8-5186-4411-8dff-e6a751f6b071)

![Screenshot 2025-03-17 162459](https://github.com/user-attachments/assets/9c59ed50-77e5-45c3-ab82-400cc076107d)


## RESULT:
The program is executed successfully and results are verified as well.
