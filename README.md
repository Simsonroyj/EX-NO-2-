## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```

#include <stdio.h>
#include <string.h>
#include <ctype.h>

char table[5][5];

void generateKey(char key[]) {
    int used[26] = {0}, k = 0;
    char s[100];

    sprintf(s, "%sabcdefghijklmnopqrstuvwxyz", key);
    used['j' - 'a'] = 1;

    for (int i = 0; s[i]; i++) {
        char c = tolower((unsigned char)s[i]);
        if (c < 'a' || c > 'z') continue;
        if (c == 'j') c = 'i';

        if (!used[c - 'a']) {
            table[k / 5][k % 5] = c;
            used[c - 'a'] = 1;
            k++;
        }
    }
}

void find(char c, int *r, int *col) {
    if (c == 'j') c = 'i';

    for (int i = 0; i < 5; i++)
        for (int j = 0; j < 5; j++)
            if (table[i][j] == c) {
                *r = i;
                *col = j;
                return;
            }
}

void encrypt(char text[]) {
    int n = strlen(text);

    for (int i = 0; i < n; i++)
        text[i] = tolower((unsigned char)text[i]);

    if (n % 2) {
        text[n++] = 'z';
        text[n] = '\0';
    }

    for (int i = 0; i < n; i += 2) {
        int r1, c1, r2, c2;
        find(text[i], &r1, &c1);
        find(text[i + 1], &r2, &c2);

        if (r1 == r2) {
            c1 = (c1 + 1) % 5;
            c2 = (c2 + 1) % 5;
        } else if (c1 == c2) {
            r1 = (r1 + 1) % 5;
            r2 = (r2 + 1) % 5;
        } else {
            int t = c1;
            c1 = c2;
            c2 = t;
        }

        text[i] = table[r1][c1];
        text[i + 1] = table[r2][c2];
    }
}

int main() {
    char key[] = "Simson";
    char text[100] = "instruments";

    printf("Key text: %s\n", key);
    printf("Plain text: %s\n", text);

    generateKey(key);
    encrypt(text);

    printf("Cipher text: %s\n", text);
    return 0;
}

```




Output:


<img width="390" height="215" alt="image" src="https://github.com/user-attachments/assets/c4c8e04b-f28a-4ed3-a5d6-d1abcd5b051d" />
