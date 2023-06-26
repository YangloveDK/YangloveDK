#include<stdio.h> 
#include<stdlib.h>
#include<string.h>
#include<windows.h> 
#include<conio.h> 
char n[5];   		//编号 
char na[20]; 	//名字 
int num;    	//场次余量
int pc;     	//价格 
typedef struct Theater {
	char id[5];					//剧目编号
	char name[20];			//剧目名称
	int sum;				//剩余量 
	int  price;				//价格
	struct Theater* next;	//指针域 

}Linklist;
struct user
{
	char username[20];
	char usermima[20];
};
//功能 
void first();
void zhuce();
void denglu();
Linklist* add_Theater(Linklist* head);        //添加剧目信息 
Linklist* delet_Theater(Linklist* head);      //删除剧目信息 
void change_Theater(Linklist* head);          //修改剧目信息 
void find(Linklist* head);                     //查询 
void findnum_Theater(Linklist* head);         //查找剧目信息(编号) 
void findname_Theater(Linklist* head);        //查找剧目信息(名称)
void sort(Linklist* head);                     //排序
void price_Theater(Linklist* head);           //按照剧目价格排序（升序） 
void number_Theater(Linklist* head);          //按照剧目场次余量排序（降序） 
void sum_Theater(Linklist* head);             //统计总剧目场次余量
void keep_Theater(Linklist* head);            //将信息保存至文件中 
void findprice_Theater(Linklist* head);       //查询剧目价格 
void insert_Theater(Linklist* head);          //插入剧目信息 
Linklist* open_Theater();                     //打开文件 
void print_Theater(Linklist* head);           //输出剧目信息

void first()
{
	printf(" ___________________________________________________________\n");
	printf("|                 欢迎使用剧院票务管理系统                      |\n");
	printf("|                        1-登录                             |\n");
	printf("|                        2-注册                             |\n");
	printf("|                        0-退出                             |\n");
	printf("|___________________________________________________________|\n");
	printf("请选择操作： ");
	int choice;
	scanf_s("%d", &choice);
	switch (choice)
	{
	case 1:
		system("cls");
		denglu();
		system("cls");
		break;
	case 2:
		system("cls");
		zhuce();
		system("cls");
		first();
		break;
	case 0:
		exit(1);
	default:first();
	}
}
void zhuce()
{

	FILE* fp;
	int i, j, flag = 0;
	char ch, bh;
	struct user a;
	char name[20];
	char code1[20];
	char code2[20];
	while (1)
	{
		printf("欢迎使用剧院票务管理系统!\n");
		printf("用户注册\n");
		printf("请输入用户名：\n");
		scanf_s("%s", name);
		if (strlen(name) > 20)
		{
			printf("用户名位数过多，请重新输入\n");
			scanf_s("%s", name);
		}
		fflush(stdin);
		fp = fopen("zhanghao.txt", "r");
		if (fp == NULL)
		{
			fp = fopen("zhanghao.txt", "w");
			fclose(fp);
			fp = fopen("zhanghao.txt", "r");
		}
		flag = 0;
		while (!feof(fp))
		{
			fscanf_s(fp, "%s %s\n", a.username, a.usermima);
			if (strcmp(a.username, name) == 0)
			{
				flag = 1;
				break;
			}
		}
		fclose(fp);
		if (flag == 1)
		{
			printf("该用户名已存在！");
			Sleep(1000);
			system("cls");
			first();
		}
		else
		{

			while (1)
			{
				printf("\n请输入密码：\n");
				while ((ch = _getch()) != '\r')//从控制台读取一个字符，但不显示在屏幕上
				{
					if (ch == '\b')
					{
						if (i != 0)
						{
							printf("\n");
							i--;
						}
					}
					else
					{
						putchar('*');
						code1[i++] = ch;
					}
				}
				code1[i] = '\0';
				i = 0;
				j = 0;
				printf("\n请再次输入密码:   ");
				while ((bh = _getch()) != '\r')
				{
					if (bh == '\b')
					{
						if (j != 0)
						{
							printf("\n");
							j--;
						}
					}
					else
					{
						putchar('*');
						code2[j++] = bh;
					}
				}
				code2[j] = '\0';
				j = 0;
				if (strcmp(code1, code2) == 0)
				{
					fp = fopen("zhanghao.txt", "a");
					fprintf(fp, "%s %s\n", name, code1);
					fclose(fp);
					break;
				}
			}
			printf("\n注册成功!\n");
			printf("返回请输入(1)\n");
			printf("输入其他键继续注册\n");
			if (getchar() == '1')
				break;
			Sleep(1000);
			system("cls");
		}
	}
}
void denglu()
{
	FILE* fp;
	int i = 0;
	char ch;
	struct user a;
	char code[20];
	char name[20];
	int	flag = 0;
	fp = fopen("zhanghao.txt", "r");
	printf("\n用户登录\n");
	fclose(fp);
	do
	{
		fp = fopen("zhanghao.txt", "r");
		printf("\n请输入用户名:");
		scanf_s("%s", name);
		if (strlen(name) > 20)
		{
			printf("用户名位数过多，请重新输入\n");
			scanf_s("%s", name);
		}
		printf("\n请输入密码:");
		while ((ch = _getch()) != '\r')
		{
			if (ch == '\b')
			{
				if (i != 0)
				{
					printf("\b \b");
					i--;
				}
			}
			else
			{
				putchar('*');
				code[i++] = ch;
			}
		}
		code[i] = '\0';
		while (!feof(fp))
		{
			fscanf_s(fp, "%s %s\n", a.username, a.usermima);
			if (strcmp(name, a.username) == 0 && strcmp(code, a.usermima) == 0)
			{
				flag = 1;
				break;
			}
		}
		fclose(fp);
		if (flag)
		{
			printf("\n登陆成功！\n");
			Sleep(1000);
			break;
		}
		else
		{
			printf("密码错误\n\n");
			printf("请重新进行选择\n");
			Sleep(1000);
			system("cls");
			first();


		}

	} while (ch == '1');
}

Linklist* add_Theater(Linklist* head) //添加剧目信息
{
	printf("请输入剧目信息：\n");
	printf("剧目编号：");
	scanf_s("%s", n);
	Linklist* q;
	q = head;
	while (q->next != NULL)
	{
		if (strcmp(q->next->id, n) == 0)
		{
			printf("此剧目已存在，无需再添加\n");
			break;
		}
		q = q->next;
	}
	if (q->next == NULL)
	{
		Linklist* p;
		p = (Linklist*)malloc(sizeof(Linklist));
		strcpy_s(p->id, n);
		printf("剧目名：");
		scanf_s("%s", p->name);
		printf("场次余量：");
		scanf_s("%d", &num);
		p->sum = num;
		printf("剧目价格：");
		scanf_s("%d", &pc);
		p->price = pc;
		p->next = q->next;
		q->next = p;
	}
	return head;
}
Linklist* delet_Theater(Linklist* head)//删除剧目信息 
{
	Linklist* q, * p;
	q = head;
	printf("请输入剧目信息:\n");
	printf("剧目编号:");
	scanf_s("%s", &n);
	while (q->next != NULL)
	{
		if (strcmp(q->next->id, n) == 0)
		{
			p = q->next;
			q->next = p->next;
			free(p);
			printf("剧目信息删除成功\n");
			break;
		}
		q = q->next;
	}
	if (q->next == NULL)
	{
		printf("没有当前剧目信息！\n");
	}
	return head;
}
void change_Theater(Linklist* head) //修改剧目信息
{
	int choice;
	Linklist* p, * q;
	int m;
	q = head;
	printf("请输入要修改的剧目信息:\n");
	printf("剧目编号:");
	scanf_s("%s", &n);
	while(q->next!=NULL)
	{
		if (strcmp(q->next->id, n) == 0)
		{
			p = q->next;
			printf("已经查询到该剧目信息:\n");
			printf("剧目序号:%s 名称:%s 场次余量:%d 价格：%d\n",
				p->id, p->name, p->sum, p->price);
			printf("----------------------------------------------------------------\n");

			printf("请选择要修改该剧目的信息\n");
			printf("1.编号   2.名称   3.场次余量   4.价格   5.全部\n");
			scanf_s("%d", &choice);
			if (choice != 1 && choice != 2 && choice != 3 && choice != 4 && choice != 5)
			{
				printf("您输入的数字有误！请重新输入。\n");
				scanf_s("%d", &choice);
			}
			if (choice == 5)
			{
				printf("请输入新的剧目信息:\n");
				printf("请输入剧目编号：");
				scanf_s("%s", &n);
				strcpy_s(p->id, n);
				printf("请输入剧目名称: ");
				scanf_s("%s", na);
				strcpy_s(p->name, na);
				printf("请输入场次余量：");
				scanf_s("%d", &num);
				p->sum = num;
				printf("请输入剧目价格：");
				scanf_s("%d", &pc);
				p->price = pc;
				printf("修改成功!\n");
				break;
			}

			if (choice == 1)
			{
				printf("请输入新的剧目信息:\n");
				printf("请输入剧目编号：");
				scanf_s("%s", &n);
				strcpy_s(p->id, n);
				printf("修改成功!\n");
				break;
			}

			if (choice == 2)
			{
				printf("请输入新的剧目信息:\n");
				printf("请输入剧目名称: ");
				scanf_s("%s", na);
				strcpy_s(p->name, na);
				printf("修改成功!\n");
				break;
			}

			if (choice == 3)
			{
				printf("请输入新的剧目信息:\n");
				printf("请输入场次余量：");
				scanf_s("%d", &num);
				p->sum = num;
				printf("修改成功!\n");
				break;
			}

			if (choice == 4)
			{
				printf("请输入新的剧目信息:\n");
				printf("请输入剧目价格：");
				scanf_s("%d", &pc);
				p->price = pc;
				printf("修改成功!\n");
				break;
			}
		}
		q = q->next;

	}
	if (q->next == NULL)
	{
		printf("没有此剧目信息!\n");
	}
}
void find(Linklist* head)//查找剧目
{
	int n;
	printf("请选择查找方式：1.按编号查找\t 2.按名称查找\t\n");
	scanf_s("%d", &n);
	switch (n)
	{
	case 1:
		findnum_Theater(head);
		break;
	case2:
		findname_Theater(head);
		break;
	default:
		printf("输入数据有误\n");
		break;
	}
}
void findnum_Theater(Linklist* head)//查找剧目信息（编号）
{
	Linklist* q=0, * p=0;
	q = head;
	printf("请输入剧目编号：");
	scanf_s("%s", &n);
	while (q->next != NULL)
	{
		if (strcmp(q->next->id, n) == 0)
		{
			p = q->next;
			printf("剧目信息如下：\n");
			printf("剧目名：%s\n", p->name);
			printf("场次余量：%d\n", p->sum);
			printf("价格:%d\n", p->price);
			break;
		}
		q = q->next;
	}
	if (p == NULL)
	{
		printf("没有当前剧目信息！");
	}
}

void findname_Theater(Linklist *head)//查找剧目信息（名称）
{
	Linklist* q=0, * p=0;
	q = head;
	printf("请输入剧目名称:\n");
	scanf_s("%s", &na);
	while (q->next != NULL)
	{
		if (strcmp(q->next->name, na) == 0)
		{
			p = q->next;
			printf("剧目信息如下：\n");
			printf("剧目编号:%s\n", p->id);
			printf("剧目名:%s\n", p->name);
			printf("场次余量:%d\n", p->sum);
			printf("价格:%d\n", p->price);
			break;
		}
		q = q->next;
	}
	if (p == NULL)
	{
		printf("没有当前剧目信息！");
	}
}
void sort(Linklist* head)//排序
{
	int n;
	printf("请选择排序放肆：1.按价格升序排序 2.按剧目场次余量降序排序 \n");
	scanf_s("%d", &n);
	switch (n)
	{
	case 1:
		price_Theater(head);
		break;
	case 2:
		number_Theater(head);
		break;
	default:
		printf("输入数据有误\n");
		break;
	}
}
void price_Theater(Linklist* head)//按价格升序（冒泡）
{
	Linklist* h, * p, * q, * c;
	h = head->next;
	if (h == NULL)
	{
		printf("没有任何药品信息");
	}
	while (h != NULL) 
	{
		p = h;
		q = h->next;
		Linklist* t;//暂存节点
		t = (Linklist*)malloc(sizeof(Linklist));
		while (q != NULL)
		{
			if (q->price < p->price)
			{
				strcpy_s(t->id, q->id);  //先复制q结点信息到t 
				strcpy_s(t->name, q->name);
				t->sum = q->sum;
				t->price = q->price;

				strcpy_s(q->id, p->id);  //复制p结点信息到q
				strcpy_s(q->name, p->name);
				q->sum = p->sum;
				q->price = p->price;

				strcpy_s(p->id, t->id);  //复制暂存结点信息到p
				strcpy_s(p->name, t->name);
				p->sum = t->sum;
				p->price = t->price;
			}
			q = q->next;
		}
		h = h->next;
	}
	printf("排序后的剧目信息为：\n");
	c = head->next;
	while (c != NULL)
	{
		printf("剧目序号;%s 名称;%s 场次余量;%d 价格;%d\n", c->id, c->name, c->sum, c->price);
		c = c->next;
	}
}
void number_Theater(Linklist* head)//按剧目场次余量排序（冒泡） 
{
	Linklist* h, * p, * q, * c;
	h = head->next;
	if (h == NULL)
	{
		printf("没有任何剧目信息");
	}
	while (h != NULL)
	{
		p = h;
		q = h->next;
		Linklist* t;  //暂存节点 
		t = (Linklist*)malloc(sizeof(Linklist));
		while (q != NULL)
		{
			if (q->sum > p->sum)
			{
				strcpy_s(t->id, q->id);  //先复制q结点信息到t 
				strcpy_s(t->name, q->name);
				t->sum = q->sum;
				t->price = q->price;

				strcpy_s(q->id, p->id);  //复制p结点信息到q
				strcpy_s(q->name, p->name);
				q->sum = p->sum;
				q->price = p->price;

				strcpy_s(p->id, t->id);  //复制暂存结点信息到p
				strcpy_s(p->name, t->name);
				p->sum = t->sum;
				p->price = t->price;
			}
			q = q->next;
		}
		h = h->next;
	}
	printf("排序后的剧目信息为：\n");
	c = head->next;
	while (c != NULL)
	{
		printf("剧目序号:%s 名称:%s 场次余量:%d 价格：%d\n",
			c->id, c->name, c->sum, c->price);
		c = c->next;
	}
}
void insert_Theater(Linklist* head)//插入剧目信息 
{
	Linklist* p, * q;
	q = head;
	p = (Linklist*)malloc(sizeof(Linklist));
	printf("请输入要插入剧目的前一个剧目编号：");
	scanf_s("%s", n);
	while (q->next != NULL)
	{
		if (strcmp(q->id, n) == 0)
		{
			printf("请输入剧目信息:\n");
			printf("剧目编号：");
			scanf_s("%s", p->id);
			printf("剧目名：");
			scanf_s("%s", p->name);
			printf("场次余量：");
			scanf_s("%d", &num);
			p->sum = num;
			printf("剧目价格：");
			scanf_s("%d", &pc);
			p->price = pc;
			p->next = q->next;
			q->next = p;
			q = p;
			printf("插入剧目信息成功\n");
			break;
		}
		q = q->next;
	}
}
void sum_Theater(Linklist* head)//统计总剧目场次余量
{
	int s = 0;
	Linklist* p;
	p = head->next;
	if (p == NULL)
	{
		printf("当前没有任何剧目信息");
	}
	while (p != NULL)
	{
		s += p->sum;
		p = p->next;
	}
	printf("总剧目场次余量为：%d\n", s);
}
void findprice_Theater(Linklist* head)//查找剧目价格 
{
	Linklist* q=0, * p=0;
	q = head;
	printf("请输入您要查找剧目的信息:\n");
	printf("请输入剧目名:");
	scanf_s("%s", &na);
	while (q->next != NULL)
	{
		if (strcmp(q->next->name, na) == 0)
		{
			p = q->next;
			printf("价格:%d\n", p->price);
			break;
		}
		q = q->next;
	}
	if (p == NULL)
	{
		printf("没有当前剧目信息！");
	}
}
void keep_Theater(Linklist* head)//将信息保存至文件中
{
	Linklist* p;
	p = head->next;
	if (p == NULL)
	{
		printf("当前没有任何剧目信息！");
	}
	while (p != NULL)
	{
		FILE* Theater = fopen("Thea.txt", "a");
		fprintf(Theater, "剧目序号:%s  名称:%s  场次余量:%d  价格：%d\n",
			p->id, p->name, p->sum, p->price);
		fclose(Theater);
		p = p->next;
	}
	printf("保存成功\n");
}
Linklist* open_Theater()//打开文件 
{
	Linklist* p, * q, * head;
	head = (Linklist*)malloc(sizeof(Linklist));
	FILE* fp;
	fp = fopen("Thea.txt", "r");
	if (fp == NULL)
	{
		printf("读取文件出错，按任意键退出!");
		exit(1);                                   //无法读取，退出程序 
	}
	if (fp != NULL)      //读取文件的内容到链表中 
	{
		head->next = NULL;
		q = head;
		while (!feof(fp))  //文件结束函数 
		{
			p = (Linklist*)malloc(sizeof(Linklist));       //申请一个新的结点，存放读取的信息
			fscanf_s(fp, "剧目序号:%s  名称:%s  场次余量:%d  价格：%d\n",  //读函数 
				p->id, p->name, &p->sum, &p->price);
			q->next = p;
			q = p;
		}
		q->next = NULL;
		fclose(fp);
		printf("\n文件信息已正确读出\n");
	}
	return head;
}
void print_Theater（(Linklist* head) {         //输出剧目信息
	Linklist* p;
	int n;
	for (p = head->next; p != NULL; p = p->next)
	{
		printf("----------------------------------------\n");
		printf("序号:%s\n", p->id);
		printf("名称:%s\n", p->name);
		printf("剩余量:%d\n", p->sum);
		printf("价格:%d\n\n", p->price);
		printf("----------------------------------------\n");
	}
}
int main()
{
	first();
	int select = 1;
	Linklist* head;
	head = (Linklist*)malloc(sizeof(Linklist));
	head->next = NULL;
	head = open_Theater();
	//	system("cls");
	while (select != 0)
	{
		printf("                              ******************************************************\n");
		printf("                              ********************剧院票务管理系统******************\n");
		printf("                              **                                                  **\n");
		printf("                              **                1. 添加剧目信息                   **\n");
		printf("                              **                2. 删除剧目信息                   **\n");
		printf("                              **                3. 修改剧目信息                   **\n");
		printf("                              **                4. 查找剧目信息                   **\n");
		printf("                              **                5. 排序剧目信息                   **\n");
		printf("                              **                6. 插入剧目信息                   **\n");
		printf("                              **                7. 查找剧目价格                   **\n");
		printf("                              **                8. 统计总剧目场次余量            **\n");
		printf("                              **                9. 将剧目信息保存到文件中         **\n");
		printf("                              **                10.打开文件                       **\n");
		printf("                              **                11.输出剧目信息                   **\n");
		printf("                              **                0. 退出                           **\n");
		printf("                              ******************************************************\n");
		printf("Select:");
		scanf_s("%d", &select);
		system("cls");
		switch (select)
		{
		case 0:
			printf("您已退出此系统，谢谢使用！");
			return 0;
		case 1:
			head = add_Theater(head);
			break;
		case 2:
			head = delet_Theater(head);
			break;
		case 3:
			change_Theater(head);
			break;
		case 4:
			find(head);
			break;
		case 5:
			sort(head);
			break;
		case 6:
			insert_Theater(head);
			break;
		case 7:
			findprice_Theater(head);
			break;
		case 8:
			sum_Theater(head);
			break;
		case 9:
			keep_Theater(head);
			break;
		case 10:
			head = open_Theater();
			break;
		case 11:
			print_Theater(head);
			break;
		default:
			break;
		}
	}

	return 0;
}
Hi, I’m @YangloveDK
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
YangloveDK/YangloveDK is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
