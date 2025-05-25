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

#define MX 5

void playfair(char ch1, char ch2, char key[MX][MX]) {
    int i, j, w, x, y, z;

    for (i = 0; i < MX; i++) {
        for (j = 0; j < MX; j++) {
            if (ch1 == key[i][j]) {
                w = i;
                x = j;
            } 
            if (ch2 == key[i][j]) {
                y = i;
                z = j;
            }
        }
    }

    if (w == y) { 
        x = (x + 1) % 5;
        z = (z + 1) % 5;
    } else if (x == z) { 
        w = (w + 1) % 5;
        y = (y + 1) % 5;
    } else { 
        int temp = x;
        x = z;
        z = temp;
    }

    printf("%c%c", key[w][x], key[y][z]); // Print directly
}

void remove_duplicates(char *str) {
    int hash[26] = {0}, i, j = 0;
    for (i = 0; str[i]; i++) {
        if (!hash[str[i] - 'A']) {
            str[j++] = str[i];
            hash[str[i] - 'A'] = 1;
        }
    }
    str[j] = '\0';
}

void prepare_input(char *str) {
    int i, j = 0;
    for (i = 0; str[i]; i++) {
        if (isalpha(str[i])) {
            str[j++] = toupper(str[i] == 'J' ? 'I' : str[i]);
        }
    }
    str[j] = '\0';
}

void generate_key_matrix(char key[MX][MX], char *keystr) {
    char alphabet[26] = "ABCDEFGHIKLMNOPQRSTUVWXYZ";  
    char temp[26] = {0};
    int i, j, k = 0;

    strcpy(temp, keystr);
    remove_duplicates(temp);

    for (i = 0; i < 26; i++) {
        if (!strchr(temp, alphabet[i])) {
            temp[strlen(temp)] = alphabet[i];
        }
    }

    k = 0;
    for (i = 0; i < MX; i++) {
        for (j = 0; j < MX; j++) {
            key[i][j] = temp[k++];
            printf("%c ", key[i][j]);
        }
        printf("\n");
    }
}

void encrypt(char *str, char key[MX][MX]) {
    printf("\nCipher Text: ");
    int i;
    for (i = 0; str[i]; i++) {
        if (str[i + 1] == '\0') 
            playfair(str[i], 'X', key);
        else {
            if (str[i] == str[i + 1]) {
                playfair(str[i], 'X', key);
            } else {
                playfair(str[i], str[i + 1], key);
                i++;
            }
        }
    }
    printf("\n");
}

int main() {
    char key[MX][MX], keystr[20], str[100];

    printf("Enter key: ");
    fgets(keystr, sizeof(keystr), stdin);
    keystr[strcspn(keystr, "\n")] = '\0'; 

    printf("Enter the plain text: ");
    fgets(str, sizeof(str), stdin);
    str[strcspn(str, "\n")] = '\0';

    prepare_input(keystr);
    prepare_input(str);

    generate_key_matrix(key, keystr);
    
    printf("\nEntered text: %s", str);
    encrypt(str, key);

    return 0;
}

```





Output:
![image](https://github.com/user-attachments/assets/214e8aaa-0b1b-439e-a136-76ad45b99870)

## RESULT:
The program is executed successfully
