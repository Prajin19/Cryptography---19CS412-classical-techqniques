# Rail Fence Cipher
Rail Fence Cipher using with different key values

### Submitted By : Prajin S
### Register Number : 212223230151

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
