# Cryptography : 19CS415 Classical Techqniques
### Submitted By : Prajin S
### Register Number : 212223230151
# Caeser Cipher
Caeser Cipher using with different key values

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

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithm. 

### Step 2:

Implementation using C code.

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:

The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>


#define SIZE 100


void toLowerCase(char str[]) { 
    for (int i = 0; str[i]; i++) 
    if (str[i] >= 'A' && str[i] <= 'Z') 
    str[i] += 32; 
    
}
int removeSpaces(char *str) { 
    int count = 0; 
    for (int i = 0; str[i]; i++) 
    if (str[i] != ' ') 
    str[count++] = str[i]; 
    str[count] = '\0'; 
    return count; 
    
}
int mod5(int num) { 
    return num % 5; 
}
int prepare(char str[]) { 
    int len = strlen(str); 
    if (len % 2) str[len++] = 'z', str[len] = '\0'; 
    return len; 
    
}
void generateKeyTable(char key[], char keyT[5][5]) {
    int dict[26] = {0}, i = 0, j = 0;
    for (int k = 0; key[k]; k++) 
    if (key[k] != 'j' && !dict[key[k] - 'a']) 
    dict[key[k] - 'a'] = 1;
    for (int k = 0; k < 26; k++) 
    if (!dict[k] && k != ('j' - 'a')) 
    dict[k] = 1;
    for (int k = 0; k < 26; k++) 
    if (dict[k]) 
    keyT[i][j++] = 'a' + k, (j == 5) && (i++, j = 0);
}
void search(char keyT[5][5], char a, char b, int pos[]) {
    if (a == 'j')
    a = 'i'; 
    if (b == 'j') 
    b = 'i';
    for (int i = 0; i < 5; i++) 
    for (int j = 0; j < 5; j++)
        if (keyT[i][j] == a) 
        pos[0] = i, pos[1] = j;
        else if (keyT[i][j] == b) 
        pos[2] = i, pos[3] = j;
}
void playfairCipher(char str[], char keyT[5][5], int enc) {
    int pos[4], len = strlen(str);
    for (int i = 0; i < len; i += 2) {
        search(keyT, str[i], str[i + 1], pos);
        if (pos[0] == pos[2]) 
        str[i] = keyT[pos[0]][mod5(pos[1] + enc)], str[i + 1] = keyT[pos[0]][mod5(pos[3] + enc)];
        else if (pos[1] == pos[3]) 
        str[i] = keyT[mod5(pos[0] + enc)][pos[1]], str[i + 1] = keyT[mod5(pos[2] + enc)][pos[1]];
        else 
        str[i] = keyT[pos[0]][pos[3]], str[i + 1] = keyT[pos[2]][pos[1]];
    }
}
void encryptDecryptPlayfair(char str[], char key[], int enc) {
    char keyT[5][5];
    toLowerCase(key), toLowerCase(str);
    removeSpaces(key), removeSpaces(str);
    prepare(str), generateKeyTable(key, keyT);
    playfairCipher(str, keyT, enc);
}
int main() {
    char key[SIZE], str[SIZE];
    printf("Enter key: "); 
    fgets(key, SIZE, stdin); 
    key[strcspn(key, "\n")] = 0;
    printf("Enter plaintext: "); 
    fgets(str, SIZE, stdin); 
    str[strcspn(str, "\n")] = 0;
    encryptDecryptPlayfair(str, key, 1);
    printf("Encrypted: %s\n", str);
    encryptDecryptPlayfair(str, key, 4);  
    printf("Decrypted: %s\n", str);
    return 0;
}

```

## OUTPUT:

Key Value : playfairbyprajin

Text to be encrypted : encryption done by Prajin

Encrypted text : cpbszoyopoitpcdwmudfho

Decrypted text : encryptiondonebypraiin

![Screenshot 2025-03-16 072732](https://github.com/user-attachments/assets/f02499a4-ce85-40c3-8dd6-31b961619654)


![Screenshot 2025-03-16 072704](https://github.com/user-attachments/assets/39357c15-b6e7-44c3-98a6-2aaf0c18c60f)


## RESULT:
The program is executed successfully and results are verified as well.


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithm. 

### Step 2:

Implementation using C code.

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:

The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
```C
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define MAX_LEN 1000
int keymat[3][3] = { {1, 2, 1}, {2, 3, 2}, {2, 2, 1} };
int invkeymat[3][3] = { {-1, 0, 1}, {2, -1, 0}, {-2, 2, -1} };
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
void remove_spaces(char *str) {
    int i, j = 0;
    for (i = 0; str[i] != '\0'; i++) {
        if (str[i] != ' ') {
            str[j++] = toupper(str[i]);
        }
    }
    str[j] = '\0'; 
}
void encode(char a, char b, char c, char *result) {
    int posa = a - 'A';
    int posb = b - 'A';
    int posc = c - 'A';
    int x = (posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0]) % 26;
    int y = (posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1]) % 26;
    int z = (posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2]) % 26;
    result[0] = key[x];
    result[1] = key[y];
    result[2] = key[z];
    result[3] = '\0';
}
void decode(char a, char b, char c, char *result) {
    int posa = a - 'A';
    int posb = b - 'A';
    int posc = c - 'A';
    int x = (posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0]);
    int y = (posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1]);
    int z = (posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2]);
    x = (x % 26 + 26) % 26;
    y = (y % 26 + 26) % 26;
    z = (z % 26 + 26) % 26;
    result[0] = key[x];
    result[1] = key[y];
    result[2] = key[z];
    result[3] = '\0';
}
int main() {
    char msg[MAX_LEN], enc[MAX_LEN] = "", dec[MAX_LEN] = "";
    int original_length;
    printf("Simulation of Hill Cipher\n");
    printf("Enter your message (uppercase only, max 1000 characters): ");
    fgets(msg, sizeof(msg), stdin);
    msg[strcspn(msg, "\n")] = 0;  
    remove_spaces(msg);
    original_length = strlen(msg); 
    for (int i = 0; i < original_length; i += 3) {
        char a = msg[i];
        char b = (i + 1 < original_length) ? msg[i + 1] : 'X'; 
        char c = (i + 2 < original_length) ? msg[i + 2] : 'X'; 
        char enc_part[4];
        encode(a, b, c, enc_part);
        strcat(enc, enc_part);
    }
    printf("Encoded message: %s\n", enc);
    for (int i = 0; i < strlen(enc); i += 3) {
        char dec_part[4];
        decode(enc[i], enc[i + 1], enc[i + 2], dec_part);
        strcat(dec, dec_part);
    }
    dec[original_length] = '\0';
    printf("Decoded message: %s\n", dec);
    return 0;
}
```


## OUTPUT:
MESSAGE TO BE ENCRYPTED: Hill cipher done by Prajin

ENCRYPTED MESSAGE: TIIFSXLHHZTLXOWKXTIRABLE

DECRYPTED MESSAGE: HILLCIPHERDONEBYPRAJIN

![Screenshot 2025-03-17 160246](https://github.com/user-attachments/assets/319a3e87-a3bb-42c7-a799-840506226f48)

![Screenshot 2025-03-17 160304](https://github.com/user-attachments/assets/69030db1-883f-494e-8806-891629f6ba5c)


## RESULT:
The program is executed successfully and results are verified as well.

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

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

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:

In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:
```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Function to encrypt using Rail Fence Cipher
void encryptRailFence(char *text, int rails, char *cipher) {
    int len = strlen(text);
    char rail[rails][len];

    // Initialize matrix with '\n' (empty placeholder)
    memset(rail, '\n', sizeof(rail));

    int row = 0, dir = 1;
    for (int i = 0; i < len; i++) {
        rail[row][i] = text[i];

        // Change direction at the top and bottom rails
        if (row == 0) {
            dir = 1; // Move down
        } else if (row == rails - 1) {
            dir = -1; // Move up
        }
        row += dir;
    }

    // Read the matrix row-wise to get the encrypted text
    int index = 0;
    for (int i = 0; i < rails; i++) {
        for (int j = 0; j < len; j++) {
            if (rail[i][j] != '\n') {
                cipher[index++] = rail[i][j];
            }
        }
    }
    cipher[index] = '\0'; // Null terminate the encrypted string
}

// Function to decrypt using Rail Fence Cipher
void decryptRailFence(char *cipher, int rails, char *decrypted) {
    int len = strlen(cipher);
    char rail[rails][len];

    // Initialize matrix with '\n' (empty placeholder)
    memset(rail, '\n', sizeof(rail));

    // Step 1: Mark the pattern positions
    int row = 0, dir = 1;
    for (int i = 0; i < len; i++) {
        rail[row][i] = '*'; // Placeholder for filling later

        if (row == 0) {
            dir = 1; // Move down
        } else if (row == rails - 1) {
            dir = -1; // Move up
        }
        row += dir;
    }

    // Step 2: Fill the marked positions with encrypted text row by row
    int index = 0;
    for (int i = 0; i < rails; i++) {
        for (int j = 0; j < len; j++) {
            if (rail[i][j] == '*' && index < len) {
                rail[i][j] = cipher[index++];
            }
        }
    }

    // Step 3: Read the decrypted text in zigzag order
    row = 0, dir = 1;
    for (int i = 0; i < len; i++) {
        decrypted[i] = rail[row][i];

        if (row == 0) {
            dir = 1;
        } else if (row == rails - 1) {
            dir = -1;
        }
        row += dir;
    }
    decrypted[len] = '\0'; // Null terminate the decrypted string
}

// Main function
int main() {
    char text[1000], encrypted[1000], decrypted[1000];
    int rails;

    printf("Enter the message to encrypt: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = 0; // Remove newline

    printf("Enter number of rails: ");
    scanf("%d", &rails);

    // Perform encryption
    encryptRailFence(text, rails, encrypted);
    printf("Encrypted Text: %s\n", encrypted);

    // Perform decryption
    decryptRailFence(encrypted, rails, decrypted);
    printf("Decrypted Text: %s\n", decrypted);

    return 0;
}
```
## OUTPUT:
Message to encrypt: Railfence by Prajin

Number of rails: 7

Encrypted Text: R ayPibrl afejecinn

Decrypted Text: Railfence by Prajin


![Screenshot 2025-03-17 204115](https://github.com/user-attachments/assets/c9dbdee8-2924-47ef-be0f-14ff1707da94)

![Screenshot 2025-03-17 204144](https://github.com/user-attachments/assets/d3fab1f6-2800-4051-814d-28c36395aebd)



## RESULT:
The program is executed successfully and results are verified as well.
