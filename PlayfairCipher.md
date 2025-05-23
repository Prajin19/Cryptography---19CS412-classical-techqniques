# PlayFair Cipher
Playfair Cipher using with different key values

### Submitted By : Prajin S
### Register Number : 212223230151

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
