# Hill Cipher
Hill Cipher using with different key values

### Submitted By : Prajin S
### Register Number : 212223230151

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
