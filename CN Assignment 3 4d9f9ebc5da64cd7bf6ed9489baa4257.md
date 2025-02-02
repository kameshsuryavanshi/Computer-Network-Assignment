# CN Assignment 3

## Name : Kameshbhai Pravinbhai Suryavanshi

## Reg. No: 23MCS0041

## Sub : CN Lab

1. Implement the following error control algorithm
a) CRC (Cyclic Redundancy Checksum)
    
    code :-           
    
    ```c
    #include <stdio.h>
    #include <string.h>
    
    #define POLYNOMIAL_LENGTH strlen(generator_poly)
    
    char transmission_data[28];
    char crc_result[28];
    char generator_poly[10];
    
    int data_length, i, j;
    
    void computeCRC();
    
    void performXOR() {
        for (j = 1; j < POLYNOMIAL_LENGTH; j++)
            crc_result[j] = ((crc_result[j] == generator_poly[j]) ? '0' : '1');
    }
    
    void receiverCheck() {
        printf("\nEnter the received data: ");
        scanf("%s", transmission_data);
        printf("\nReceived Data: %s", transmission_data);
    
        computeCRC();
    
        for (i = 0; (i < POLYNOMIAL_LENGTH - 1) && (crc_result[i] != '1'); i++);
    
        if (i < POLYNOMIAL_LENGTH - 1)
            printf("\nError Detected\n\n");
        else
            printf("\nNo Error Detected\n\n");
    }
    
    void computeCRC() {
        for (i = 0; i < POLYNOMIAL_LENGTH; i++)
            crc_result[i] = transmission_data[i];
    
        do {
            if (crc_result[0] == '1')
                performXOR();
    
            for (j = 0; j < POLYNOMIAL_LENGTH - 1; j++)
                crc_result[j] = crc_result[j + 1];
    
            crc_result[j] = transmission_data[i++];
        } while (i <= data_length + POLYNOMIAL_LENGTH - 1);
    }
    
    int main() {
        printf("\nEnter data to be transmitted: ");
        scanf("%s", transmission_data);
        printf("\nEnter the Generator Polynomial: ");
        scanf("%s", generator_poly);
    
        data_length = strlen(transmission_data);
    
        for (i = data_length; i < data_length + POLYNOMIAL_LENGTH - 1; i++)
            transmission_data[i] = '0';
        printf("\nData Padded with n-1 Zeros: %s", transmission_data);
    
        computeCRC();
    
        printf("\nCRC or Check Value: %s", crc_result);
    
        for (i = data_length; i < data_length + POLYNOMIAL_LENGTH - 1; i++)
            transmission_data[i] = crc_result[i - data_length];
    
        printf("\nFinal Data to be Sent: %s", transmission_data);
    
        receiverCheck();
    
        return 0;
    }
    ```
    
     output :- 
    
    ![Untitled](Untitled.png)
    
    b) Checksum
    
    ```c
    #include <stdio.h>
    #include <stdint.h>
    #include <stdlib.h>
    
    uint16_t calculateChecksum(const uint8_t *data, size_t length) {
        uint32_t sum = 0;
        while (length > 1) {
            sum += *((uint16_t *)data);
            data += 2;
            length -= 2;
        }
        if (length == 1) {
            sum += *((uint8_t *)data);
        }
        while (sum >> 16) {
            sum = (sum & 0xFFFF) + (sum >> 16);
        }
        return (uint16_t)~sum;
    }
    
    void printChecksumBinary(uint16_t checksum) {
        printf("Checksum (binary): ");
        for (int i = 15; i >= 0; i--) {
            printf("%d", (checksum >> i) & 1);
        }
        printf("\n");
    }
    
    int main() {
        const size_t MAX_DATA_LENGTH = 100;
        uint8_t user_data[MAX_DATA_LENGTH];
        size_t data_length;
    
        printf("Enter the length of data (in bytes): ");
        scanf("%zu", &data_length);
        if (data_length > MAX_DATA_LENGTH) {
            printf("Input data exceeds maximum length.\n");
            return 1;
        }
    
        printf("Enter the data in binary format (e.g., 01010101):\n");
        for (size_t i = 0; i < data_length; ++i) {
            scanf("%1hhu", &user_data[i]);
        }
    
        uint16_t checksum = calculateChecksum(user_data, data_length);
        printChecksumBinary(checksum);
    
        printf("\nSimulating reception without error...\n");
        uint16_t received_checksum_without_error = calculateChecksum(user_data, data_length);
        printChecksumBinary(received_checksum_without_error);
        if (received_checksum_without_error == checksum) {
            printf("Checksum verification passed. Data received without errors.\n");
        } else {
            printf("Checksum verification failed. Data corrupted.\n");
        }
    
        printf("\nSimulating reception with an error in data...\n");
        user_data[data_length / 2] ^= 1;
        uint16_t received_checksum_with_error = calculateChecksum(user_data, data_length);
        printChecksumBinary(received_checksum_with_error);
        if (received_checksum_with_error == checksum) {
            printf("Checksum verification passed. Data received without errors.\n");
        } else {
            printf("Checksum verification failed. Data corrupted.\n");
        }
    
        return 0;
    }
    ```
    
    output :- 
    
    ![Untitled](Untitled%201.png)