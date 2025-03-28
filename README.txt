#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>

#define MAX_STUDENTS 100
#define MAX_NAME_LEN 50
#define MAX_ID_LEN 20
#define MAX_COURSE_LEN 50

typedef struct {
    char name[MAX_NAME_LEN];
    char id[MAX_ID_LEN];
    char course[MAX_COURSE_LEN];
    float score;
} Student;

Student students[MAX_STUDENTS];
int studentCount = 0;

void displayMenu() {
    printf("\n=================================\n");
    printf("    欢迎使用学生成绩管理系统\n");
    printf("=================================\n");
    printf("1. 记录学生成绩\n");
    printf("2. 查询学生成绩\n");
    printf("3. 统计课程成绩\n");
    printf("4. 退出系统\n");
    printf("请输入选项序号：");
}

void addStudent() {
    if (studentCount >= MAX_STUDENTS) {
        printf("错误：学生数量已达上限!\n");
        return;
    }

    Student newStudent;
    char buffer[100];

    printf("\n===== 记录学生成绩 =====\n");

    printf("请输入学生姓名：");
    fgets(newStudent.name, MAX_NAME_LEN, stdin);
    newStudent.name[strcspn(newStudent.name, "\n")] = '\0';

    do {
        printf("请输入学生学号：");
        fgets(buffer, MAX_ID_LEN, stdin);
        buffer[strcspn(buffer, "\n")] = '\0';
        
        for (int i = 0; i < studentCount; i++) {
            if (strcmp(students[i].id, buffer) == 0) {
                printf("错误：学号已存在!\n");
                buffer[0] = '\0';
                break;
            }
        }
    } while (buffer[0] == '\0');
    strcpy(newStudent.id, buffer);

    printf("请输入课程名称：");
    fgets(newStudent.course, MAX_COURSE_LEN, stdin);
    newStudent.course[strcspn(newStudent.course, "\n")] = '\0';

    int valid = 0;
    do {
        printf("请输入成绩(0-100):");
        fgets(buffer, sizeof(buffer), stdin);
        if (sscanf(buffer, "%f", &newStudent.score) != 1) {
            printf("错误：无效输入!\n");
        } else if (newStudent.score < 0 || newStudent.score > 100) {
            printf("错误：成绩超出范围!\n");
        } else {
            valid = 1;
        }
    } while (!valid);

    students[studentCount++] = newStudent;
    printf("成绩已成功记录！\n");
}

void queryStudent() {
    printf("\n===== 查询学生成绩 =====\n");
    printf("1. 按姓名查询\n");
    printf("2. 按学号查询\n");
    printf("3. 按课程查询\n");
    printf("请选择查询方式：");
    
    int choice;
    char buffer[100];
    fgets(buffer, sizeof(buffer), stdin);
    sscanf(buffer, "%d", &choice);

    printf("请输入查询内容：");
    char keyword[100];
    fgets(keyword, sizeof(keyword), stdin);
    keyword[strcspn(keyword, "\n")] = '\0';

    int found = 0;
    for (int i = 0; i < studentCount; i++) {
        int match = 0;
        switch(choice) {
            case 1: 
                match = (strcmp(students[i].name, keyword) == 0);
                break;
            case 2:
                match = (strcmp(students[i].id, keyword) == 0);
                break;
            case 3:
                match = (strcmp(students[i].course, keyword) == 0);
                break;
            default:
                printf("无效选项!\n");
                return;
        }
        
        if (match) {
            printf("[记录%d] 姓名：%s 学号：%s\n  课程：%s 成绩：%.1f\n",
                   found+1, students[i].name, students[i].id, 
                   students[i].course, students[i].score);
            found++;
        }
    }
    
    if (!found) printf("未找到相关记录!\n");
}

void statCourse() {
    printf("\n===== 统计课程成绩 =====\n");
    printf("请输入课程名称：");
    char course[MAX_COURSE_LEN];
    fgets(course, MAX_COURSE_LEN, stdin);
    course[strcspn(course, "\n")] = '\0';

    float sum = 0, max = -1, min = 101;
    int count = 0;
    
    for (int i = 0; i < studentCount; i++) {
        if (strcmp(students[i].course, course) == 0) {
            sum += students[i].score;
            count++;
            if (students[i].score > max) max = students[i].score;
            if (students[i].score < min) min = students[i].score;
        }
    }

    if (count == 0) {
        printf("该课程无记录!\n");
        return;
    }

    printf("课程：%s\n", course);
    printf("平均分：%.2f  最高分：%.1f  最低分：%.1f\n", 
           sum/count, max, min);
}

int main() {
    int running = 1;
    while(running) {
        displayMenu();
        
        char input[10];
        fgets(input, sizeof(input), stdin);
        int choice;
        if (sscanf(input, "%d", &choice) != 1) {
            printf("错误：请输入数字选项!\n");
            continue;
        }

        if (strchr(input, '\n') == NULL)
            while(getchar() != '\n');

        switch(choice) {
            case 1: addStudent(); break;
            case 2: queryStudent(); break;
            case 3: statCourse(); break;
            case 4: 
                printf("\n感谢使用学生成绩管理系统,再见！\n");
                running = 0;
                break;
            default:
                printf("无效选项!\n");
        }
    }

    /* 保持终端停留 */
    printf("按回车键退出程序...");
    while(getchar() != '\n');  // 清空缓冲区
    getchar();                // 等待最终输入
    system("pause");
    return 0;
}
