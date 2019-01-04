#include<stdio.h>
#include<Windows.h>

#define SN 100   //学生人数
#define CN  3   //科目数
#define NL 20   //名字字节长度

typedef struct student{
    long int num;
    char name[NL];
    float score[CN];
    float sum;
    float aver;
}STU;

STU s[SN];
int itemSelected=-1;
int n=0;//学生数
int m=0;//课程数

int Menu();//菜单
void chose(int itemSelected);//选择
void inputScore();//输入分数
void AverofC();//计算每门课程平均数
void sortByNum();//学生总分排行
void sortByGrade();
void findByNum();
void statisticsAnalysis();
void printScore();

int main(){
    printf("请输入学生总数(n<%d):\n",SN);
    scanf("%d",&n);
    if(n>SN){
    	printf("人数错误！");
		return 0; 
	}
    m= CN;
    while(1){
    itemSelected=Menu();
    chose(itemSelected);
    itemSelected=-1;
    }
    return 0;
}

int Menu(){
    int itemSelected;
    system("cls");
    printf("\n***********************************\n");
    printf("\t 1.录入学生信息\n");
    printf("\t 2.课程总分及平均分\n");
    printf("\t 3.按成绩排序\n");
    printf("\t 4.按学号升序排列\n");
    printf("\t 5.按学号查询学生排名和考试成绩\n");
    printf("\t 6.统计比例\n");
    printf("\t 7.显示输出学生信息\n");
    printf("\t 0.退出\n");
    printf("****************************************\n");
    printf("请输入功能号:  ");
    scanf("%d",&itemSelected);
    return itemSelected;
}

void chose(int itemSelected){
    switch(itemSelected){
    case 1: inputScore(); break;
    case 2: AverofC();break;
    case 3: sortByGrade();break;
    case 4: sortByNum();break;
    case 5: findByNum();break;
    case 6: statisticsAnalysis();break;
    case 7: printScore();break;
    case 0: exit(0);
        printf("即将退出程序");break;
    default: printf("输入错误!");break;
    }
}

void inputScore(){
    printf("\n*****************************************\n");
    printf("请输入学生学号、姓名及各科信息(输入0停止信息输入)\n");
    for(int i=0;i<n;i++){
        printf("学号(11位)");
        scanf("%ld",&s[i].num);
        if(s[i].num==0) break;
        getchar();//吸收多余换行符
        printf("姓名: ");
        gets(s[i].name);
        printf("请输入各科成绩: \n");
        s[i].sum=0;
        for(int j =0;j<m;j++){
        	switch(j)
        	{
        		case 0: printf("数学成绩："); break;
				case 1: printf("语文成绩："); break;
				case 2: printf("英语成绩："); break; 
			}	
            scanf("%f",&s[i].score[j]);
            s[i].sum +=s[i].score[j];
        }
    }
}

void AverofC(){
    float sum[CN]={0};
    float average[CN]={0};
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
        sum[i]+=s[j].score[i];
        }
        average[i]=sum[i]/n;
        if(i==0)
        printf("数学课程的总分为：%.2f，平均分为%.2f\n",sum[i],average[i]);
        if(i==1)
        printf("语文课程的总分为：%.2f，平均分为%.2f\n",sum[i],average[i]);
        if(i==2)
		printf("英语课程的总分为：%.2f，平均分为%.2f\n",sum[i],average[i]); 
    }       
    getchar();
}

void sortByNum(){
    //按学号从小到大排序
    STU temp1={0};
     for(int i=0;i<n-1;i++){
        if(s[i].num>s[i+1].num){
            temp1=s[i];
            s[i]=s[i+1];
            s[i+1]=temp1;
        }
    }
    printf("\n**************************按学号从小到排序************************\n");
    for(int j=0;j<n;j++){
        printf("该学生的学号为:%d\n",s[j].num);
        printf("该学生的姓名为:");
        for(int k=0;k<NL;k++){
            printf("%c",s[j].name[k]);
        }
        printf("\n该学生的各科成绩为\n");
        for(int c=0;c<m;c++){
            printf("第%d门成绩为%.2f\n",c+1,s[j].score[c]);
        }
    printf("\n");
    }
    getchar();
}

void sortByGrade(){
	int z=0,k=-1;
	printf("\n**************************************\n");
	printf("0.数学\n1.语文\n2.英语\n3.总分\n");
	printf("\n**************************************\n");
	printf("按哪个属性排序： ");
	scanf("%d",&k) ;
	if(k!=0&&k!=1&&k!=2&&k!=3)
	{
		printf("输入错误！");
		return;
	}
	printf("\n**************************************\n");
	
	printf("1.升序\n2.降序\n请输入功能号: \n") ;
	scanf("%d",&z);
	//总分 
	if(k==3)
	{
		if(z==1)
	{
		//总分按由低到高排序 
		STU temp1={0};
     for(int i=0;i<n-1;i++){
         if(s[i].sum>s[i+1].sum){
            temp1=s[i];
            s[i]=s[i+1];
            s[i+1]=temp1;
        }
    }
    printf("\n*************************按总成绩从低到高排序*********************\n");
    for(int j=0;j<n;j++){
        printf("该学生的学号为:%d\n",s[j].num);
        printf("该学生的姓名为:");
        for(int k=0;k<NL;k++){
            printf("%c",s[j].name[k]);
        }
        printf("\n该学生的各科成绩为\n");
        for(int c=0;c<m;c++){
            printf("第%d门成绩为%.2f\n",c+1,s[j].score[c]);
        }
        printf("该学生的总成绩为:%.2f",s[j].sum);
        printf("\n");
    }
    getchar();
	}
	else if(z==2){
		 //按总分从高到低排序
    STU temp1={0};
     for(int i=0;i<n-1;i++){
         if(s[i].sum<s[i+1].sum){
            temp1=s[i];
            s[i]=s[i+1];
            s[i+1]=temp1;
        }
    }
    printf("\n****************************按总成绩从高到低排序**********************\n");
    for(int j=0;j<n;j++){
        printf("该学生的学号为:%d\n",s[j].num);
        printf("该学生的姓名为:");
        for(int k=0;k<NL;k++){
            printf("%c",s[j].name[k]);
        }
        printf("\n该学生的各科成绩为\n");
        for(int c=0;c<m;c++){
            printf("第%d门成绩为%.2f\n",c+1,s[j].score[c]);
        }
        printf("该学生的总成绩为:%.2f",s[j].sum);
        printf("\n");
    }
    getchar();
	}
	}
	//各科 
	else 
	{
		if(z==1)
	{
		//按由低到高排序 
		STU temp1={0};
     for(int i=0;i<n-1;i++){
         if(s[i].score[k]>s[i+1].score[k]){
            temp1=s[i];
            s[i]=s[i+1];
            s[i+1]=temp1;
        }
    }
    printf("\n******************************按成绩从低到高排序******************************\n");
    for(int j=0;j<n;j++){
        printf("该学生的学号为:%d\n",s[j].num);
        printf("该学生的姓名为:");
        for(int k=0;k<NL;k++){
            printf("%c",s[j].name[k]);
        }
        printf("\n该学生的各科成绩为\n");
        for(int c=0;c<m;c++){
            printf("第%d门成绩为%.2f\n",c+1,s[j].score[c]);
        }
        printf("该学生的总成绩为:%.2f",s[j].sum);
        printf("\n");
    }
    getchar();
	}
	else if(z==2){
		 //按从高到低排序
    STU temp1={0};
     for(int i=0;i<n-1;i++){
         if(s[i].score[k]<s[i+1].score[k]){
            temp1=s[i];
            s[i]=s[i+1];
            s[i+1]=temp1;
        }
    }
    printf("\n*************************按成绩从高到低排序********************\n");
    for(int j=0;j<n;j++){
        printf("该学生的学号为:%d\n",s[j].num);
        printf("该学生的姓名为:");
        for(int k=0;k<NL;k++){
            printf("%c",s[j].name[k]);
        }
        printf("\n该学生的各科成绩为\n");
        for(int c=0;c<m;c++){
            printf("第%d门成绩为%.2f\n",c+1,s[j].score[c]);
        }
        printf("该学生的总成绩为:%.2f",s[j].sum);
        printf("\n");
    }
    getchar();
	}
	 } 
	
   
}

void findByNum() {
    int find=-1;
    //记录需要查找的学号,并作为查找成功与否的标识符
    printf("请输入要查找的学号：");
    scanf("%d",&find);
    for(int i=0;i<n;i++){
        if(s[i].num==find){
            printf("\n查找成功\n");
            printf("该学生的学号为%d\n",s[i].num);
            printf("该学生的姓名为:");
            for(int k=0;k<NL;k++){
                printf("%c",s[i].name[k]);
            }
            printf("\n该学生的各科成绩为\n");
            for(int c=0;c<m;c++){
                printf("第%d门成绩为%.2f\n",c+1,s[i].score[c]);
            }
            printf("该学生的总成绩为:%.2f",s[i].sum);
            printf("\n");
            find=-2;
        }
    }
    if(find!=-2){
        printf("查无此人\n");
        printf("按任意键继续");
    }
    getchar();
}


void statisticsAnalysis(){
    printf("输出每门课程优秀、良好、中等、及格、不及格人数所占的百分比\n");
    int a1[CN]={0},a2[CN]={0},a3[CN]={0},a4[CN]={0},a5[CN]={0};
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            if(s[i].score[j]>=90 && s[i].score[j]<=100){
                a1[j]++;
            }else if (s[i].score[j]>=80 && s[i].score[j]<90){
                a2[j]++;
            }else if (s[i].score[j]>=70 && s[i].score[j]<80){
                a3[j]++;
            }else if (s[i].score[j]>=60 && s[i].score[j]<70){
                a4[j]++;
            }else if (s[i].score[j]<60){
                a5[j]++;
            }
        }
    }
    printf("\n*************************各们课程概况**************************\n");
        for(int k=0;k<m;k++){
            printf("第%d门课的优秀人数占%.2f%%，",k+1,(a1[k]/(n*1.0))*100);
            printf("良好人数占%.2f%%，",(a2[k]/(n*1.0))*100);
            printf("中等人数占%.2f%%，",(a3[k]/(n*1.0))*100);
            printf("及格人数占%.2f%%，",(a4[k]/(n*1.0))*100);
            printf("不及格人数占%.2f%%，",(a5[k]/(n*1.0))*100);

            printf("\n");
        }
    getchar();
}

void printScore(){
    printf("\n**************************以下为学生信息**********************\n");
    for(int j=0;j<n;j++){
        printf("该学生的学号为:%d\n",s[j].num);
        printf("该学生的姓名为:");
        for(int k=0;k<NL;k++){
            printf("%c",s[j].name[k]);
        }
        printf("\n该学生的各科成绩为\n");
        for(int c=0;c<m;c++){
            printf("第%d门成绩为%.2f\n",c+1,s[j].score[c]);
        }
        printf("该学生的总成绩为:%.2f",s[j].sum);
        printf("\n");
    }
    getchar();
}
