# EXP-14-IMPLEMENT-HASH-FUNCTION-CRYPTOGRAPHY

## AIM:
To write a program to implement Hash Algorithm. 

## ALGORITHM: 
STEP-1: Input the plaintext message and symmetric key. 
STEP-2: Perform XOR encryption 
STEP-3: HMAC calculation. 
STEP-4: Display the encrypted message 
STEP-5: Perform XOR decryption 
  STEP-6: Display the decrypted message 
  
## PROGRAM: 
```
#include <stdio.h>
#include <string.h>

#define MAX_LEN 256       
#define BLOCK_SIZE 64    


void xor_pad(const char *key, char pad, char *output, int key_len) {
    for (int i = 0; i < key_len; i++) {
        output[i] = key[i] ^ pad;
    }
    for (int i = key_len; i < BLOCK_SIZE; i++) {
        output[i] = pad;
    }
}


void simple_hash(const char *input, char *output) {
    int len = strlen(input);
    char hash_value = 0;
    for (int i = 0; i < len; i++) {
        hash_value ^= input[i];
    }
    snprintf(output, 3, "%02x", hash_value);
}


void hmac(const char *message, const char *key, char *output_mac) {
    char o_key_pad[BLOCK_SIZE];    
    char i_key_pad[BLOCK_SIZE];   
    char temp[MAX_LEN + BLOCK_SIZE]; 
    char inner_hash[3];          
    int key_len = strlen(key);

    
    xor_pad(key, 0x36, i_key_pad, key_len);
    xor_pad(key, 0x5c, o_key_pad, key_len);


    strcpy(temp, i_key_pad);
    strcat(temp, message);
    simple_hash(temp, inner_hash); 

    
    strcpy(temp, o_key_pad);
    strcat(temp, inner_hash);
    simple_hash(temp, output_mac); 
}


void encrypt(const char *input, const char *key, char *output) {
    int len = strlen(input);
    int key_len = strlen(key);
    for (int i = 0; i < len; i++) {
        output[i] = input[i] ^ key[i % key_len]; // XOR encryption
    }
    output[len] = '\0'; 


void decrypt(const char *input, const char *key, char *output) {
    encrypt(input, key, output); // XOR encryption is symmetric
}

int main() {
    char message[MAX_LEN];   
    char key[MAX_LEN];        
    char mac[3];              
    char encrypted[MAX_LEN]; 
    char decrypted[MAX_LEN];

    printf("\n **Simulation of HMAC Algorithm with Encryption and Decryption**\n\n");
    printf("Gokul J 212222230038\n");
   
    printf("Enter the plaintext message: ");
    fgets(message, MAX_LEN, stdin);
    message[strcspn(message, "\n")] = 0;

  
    printf("Enter the symmetric key: ");
    fgets(key, MAX_LEN, stdin);
    key[strcspn(key, "\n")] = 0; 

   
    hmac(message, key, mac);
    printf("Generated HMAC: %s\n", mac);

   
    encrypt(message, key, encrypted);
    printf("Encrypted message (raw bytes): ");
    for (int i = 0; i < strlen(message); i++) {
        printf("%02x ", (unsigned char)encrypted[i]);
    }
    printf("\n");

   
    decrypt(encrypted, key, decrypted);
    printf("Decrypted message: %s\n", decrypted);

    return 0;
}



```
## OUTPUT: 

![image](https://github.com/user-attachments/assets/dbb6be8f-1720-4b37-97d2-06cdc3e72495)



## RESULT: 
Thus the Hash-based Message Authentication Code is implemented successfully.
