# c
#include <stdio.h>
#include <string.h>
void registers();
void Input_login();
void Create_File();

typedef struct usermessage
{
    char id[10];
    char key[20];
    char name[15];
    char sex;
    long phonenumber;
    char bookname[20];
    char booknumber[10];
}users;

int main()
{
    int a;
    printf("***************************************************\n");
    printf("***************************************************\n");
    printf("欢迎来到图书馆管理系统，请输入数字进行下一步操作\n");
    printf("1、登录 2、注册 \n");
    printf("***************************************************\n");
    printf("***************************************************\n");
    scanf("%d",&a);
    switch(a)
    {
        case 1:
            Input_login();
            break;
        case 2:
            registers();
            break;
        default:
            printf("你的输入有误，请再次输入！\n");
            registers();
            break;

    }
    return 0;
}



void Create_File()  //创建用来储存用户账号密码的文件
{
    FILE *fp;
    if((fp=fopen("users.txt","rb"))==NULL)
    {
        if((fp=fopen("users.txt","wb+"))==NULL)
        {
          printf("无法创立文件！\n");
          exit(0);
        }
    }
}

//注册账号

void registers()
{
    users a,b;
    FILE *fp;
    char temp[20];
    printf("==================================================================================================================\n");
    printf("************欢迎来到注册页面************\n");
    printf("==================================================================================================================\n");
    fp=fopen("users.txt","r");
    fread(&b,sizeof(struct usermessage),1,fp);//读入一个结构体字符串到b
    printf("请输入账号\n");
    scanf("%s",&a.id);
     while(1)
    {
        if(strcmp(a.id,b.id))
        {
        if(!feof(fp))
            {
                fread(&b,sizeof(struct usermessage),1,fp);
            }
        else
            break;
       }
       else
        {
        printf("该用户已存在\n");
        fclose(fp);
        printf("是否继续注册?\n(Y/N)");
        if(getch()=='Y') return registers();
        else return Input_login();
        }
}

    printf("请输入姓名：\n");
    scanf("%s",&a.name);
    getchar();
    printf("请输入性别(M(男）/W（女）)：\n");
    scanf("%c",&a.sex);
    printf("请输入电话号码：\n");
    scanf("%ld",&a.phonenumber);
    printf("请输入密码：\n");
    scanf("%s",&a.key);
    printf("请再次输入密码：\n");
    scanf("%s",&temp);

    //两次输入密码一致性检查
    if(strcmp(a.key,temp)!=0)
        {
        printf("两次密码不一致，请重新输入！\n");
        scanf("%s",&a.key);
        printf("请再次输入密码：\n");
        scanf("%s",&temp);
        }
    else{
        fp=fopen("users.txt","a");
        fwrite(&a,sizeof(struct usermessage),1,fp);
        printf("账号注册成功！\n");
         }

    return Input_login();

}

//登录系统

void Input_login()
{
    #define cishu 5
    int i=0,count=cishu;
    users a,b;
    FILE *fp;
    printf("==================================================================================================================\n");
    printf("========================================\n");
    printf("*********** 欢迎来到登录页面！***********\n");
    printf("=========================================\n");
    printf("==================================================================================================================\n");
    fp=fopen("users.txt","r");
    fread(&b,sizeof(struct usermessage),1,fp);
    printf("请输入账号\n");
    scanf("%s",&a.id);
    while(1)
    {
        if(strcmp(a.id,b.id)==0)
        {
            break;
        }
        else
        {
            if(!feof(fp))
            {
                fread(&b,sizeof(struct usermessage),1,fp);
            }
            else
            {
                printf("此用户不存在，请重新输入！\n");
                fclose(fp);
                return;
            }
        }
    }

    printf("请输入密码：\n");
    scanf("%s",&a.key);
    do{
        if(strcmp(a.key,b.key)==0){
            fclose(fp);
            printf("登录成功，欢迎开始使用！\n");
            break;
        }
        else{
            printf("密码有误，请重新输入(共有 %d 次机会)\n",cishu);
            printf("（还有 %d 次重新输入机会）\n",count);
            scanf("%s",&a.key);
            count--;
            i++;
        }
    }while( i<cishu );
}



