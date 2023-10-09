#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>
#include <ctype.h>
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

// 密码强度评估函数
int evaluatePasswordStrength(char *password) {
    int length = strlen(password);
    int hasDigit = 0;
    int hasLower = 0;
    int hasUpper = 0;
    
    if (length < 8) {
        return 1; // 弱密码
    }
    
    for (int i = 0; i < length; i++) {
        if (isdigit(password[i])) {
            hasDigit = 1;
        } else if (islower(password[i])) {
            hasLower = 1;
        } else if (isupper(password[i])) {
            hasUpper = 1;
        }
    }
    
    if (hasDigit && hasLower && hasUpper) {
        return 3; // 强密码
    } else if (hasDigit || hasLower || hasUpper) {
        return 2; // 中等密码
    } else {
        return 1; // 弱密码
    }
}

// 生成密码函数
void generatePassword(char *password, int length) {
    srand(time(NULL));
    
    for (int i = 0; i < length; i++) {
        int type = rand() % 3; // 随机选择数字、小写字母或大写字母
        
        if (type == 0) {
            password[i] = '0' + rand() % 10; // 生成数字字符
        } else if (type == 1) {
            password[i] = 'a' + rand() % 26; // 生成小写字母字符
        } else {
            password[i] = 'A' + rand() % 26; // 生成大写字母字符
        }
    }
    
    password[length] = '\0'; // 添加字符串结束符
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
    char password[17]; // 限定密码长度不超过17
    int width = 125; // 设定输出宽度
    printCentered("==============================", width); 
    printCentered("欢迎使用密码管理系统", width);
    printCentered("==============================", width);
    printf("请选择操作：\n");
    printf("1. 加密\n");
    printf("2. 解密\n");
    printf("3. 判断密码强度\n");
    printf("4. 密码生成\n");
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
        case 3:
            printf("请输入密码: ");
            scanf("%s", password);
            
            int strength = evaluatePasswordStrength(password);
            printf("密码强度: ");
            
            switch (strength) 
            {
                case 1:
                    printf("弱\n");
                    break;
                case 2:
                    printf("中等\n");
                    break;
                case 3:
                    printf("强\n");
                    break;
                default:
                    printf("未知\n");
                    break;
            }
            break;
                case 4:
            printf("请输入要生成的密码长度: ");
            int length;
            scanf("%d", &length);
            
            char generatedPassword[length + 1]; // 生成的密码
            
            generatePassword(generatedPassword, length);
            printf("生成的密码: %s\n", generatedPassword);
            break;
        default:
            printf("无效的操作编号\n");
            break;
            }
    system("pause");
    return 0;
}