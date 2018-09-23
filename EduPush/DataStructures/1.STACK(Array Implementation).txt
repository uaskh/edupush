#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 100000


struct StackData
{
    int STK[MAXSIZE];
    int top;
};

typedef struct StackData *PointertoStack;
typedef PointertoStack STACK;

STACK CreateStack()
{
    STACK temp;

    temp=(STACK)malloc(sizeof(struct StackData));

    if(temp==NULL)
        printf("ERROR...Out of memory\n");

    else
    {
        temp->top=-1;
    }

    return temp;

}


int IsEmpty(STACK S)
{
    return (S->top==-1);
}

int IsFull(STACK S)
{
    return (S->top==MAXSIZE-1);
}

int Top(STACK S)
{
    if(!IsEmpty(S))
    return (S->STK[S->top]);

    else
        return(-999);
}

void Pop(STACK S)
{
    if(!IsEmpty(S))
    (S->top)--;
}

void Push(int X,STACK S)
{
    if(!IsFull(S))
    (S->STK[++(S->top)])=X;
}

void MakeEmpty(STACK S)
{
    S->top=-1;
}

void PrintStack(STACK S)
{
    int i;
    for(i=S->top;i>=0;i--)
    {
       printf("%d\n",S->STK[i]);
    }

    printf("-------------------\n");
}

//####################################################################################################################3



































void PrintMenu()
{
    printf("\n--------------------\n");
    printf("Press 1 to see TOP\n");
    printf("Press 2 to PUSH\n");
    printf("Press 3 to POP\n");
    printf("Press 4 to Empty Stack\n");
    printf("\nPress 0 to EXIT\n");
    printf("-----------------------\n");
}



int main()
{
    STACK S;

    S=CreateStack();

   // printf("%u\n",L);

    int i;   int X,Y;
    char C;

    do
    {
        system("cls");

        PrintStack(S);
        PrintMenu();

        C=getch();

       switch(C)
       {
           case '1': {
                         X=Top(S);
                         printf("On TOP== %d\n",X);
                        } break;

           case '2':  {

                        printf("\n Enter no. to PUSH\n");
                        scanf("%d",&X);
                        Push(X,S);
                       }break;

           case '3':  {
                         X=Top(S);

                         Pop(S);
                         system("cls");
                         PrintStack(S);

                          printf("%d POPED\n",X);

                          getche();

                       }  break;


           case '4':  MakeEmpty(S);  break;


            case '0':  {

                          printf("BYEEEEEEE\n");
                          exit(0);
                       }

       }

    }while(C!='0');

    return 0;

}
