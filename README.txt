#include <stdio.h>
#include <string.h>
#include <stdlib.h>
// 加密函数
void encrypt(char *password) {
    int length = strlen(password);
    
    for (int i = 0; i < length; i++) {
        // 将每个字符的ASCII码加上位置和偏移值
        password[i] += (i + 1) + 3;
    }
    
    // 调换第一位和最后一位
    char temp = password[0];
    password[0] = password[length - 1];
    password[length - 1] = temp;
    
    // 反转字符串
    int start = 0;
    int end = length - 1;
    
    while (start < end) {
        temp = password[start];
        password[start] = password[end];
        password[end] = temp;
        start++;
        end--;
    }
}

// 解密函数
void decrypt(char *password) {
    int length = strlen(password);
    
    // 反转字符串
    int start = 0;
    int end = length - 1;
    
    while (start < end) {
        char temp = password[start];
        password[start] = password[end];
        password[end] = temp;
        start++;
        end--;
    }
    
    // 调换第一位和最后一位
    char temp = password[0];
    password[0] = password[length - 1];
    password[length - 1] = temp;
    
    for (int i = 0; i < length; i++) {
        // 将每个字符的ASCII码减去位置和偏移值
        password[i] -= (i + 1) + 3;
    }
}

void printCentered(char *text, int width) {
    int length = strlen(text);
    int padding = (width - length) / 2;
    
    for (int i = 0; i < padding; i++) {
        printf(" ");
    }
    
    printf("%s\n", text);
}

int main() {
    int choice;
    char password[17]; // 限定密码长度不超过16
    int width = 125; // 设定输出宽度
    printCentered("==============================", width);
    printCentered("欢迎使用密码管理系统", width);
    printCentered("==============================", width);
    printf("请选择操作：\n");
    printf("1. 加密\n");
    printf("2. 解密\n");
    printf("请输入操作编号: ");
    scanf("%d", &choice);
    
    switch (choice) {
        case 1:
            printf("请输入密码: ");
            scanf("%s", password);
            
            encrypt(password);
            printf("加密后的密码: %s\n", password);
            break;
        case 2:
            printf("请输入密码: ");
            scanf("%s", password);
            
            decrypt(password);
            printf("解密后的密码: %s\n", password);
            break;
        default:
            printf("无效的操作编号\n");
            break;
    }
    system("pause");
    return 0;
}


//这是增加的内容