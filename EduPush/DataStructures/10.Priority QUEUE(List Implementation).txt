#include <stdio.h>
#include <stdlib.h>
#include <windows.h>

typedef struct NODE *PointertoNode;
typedef PointertoNode LIST;
typedef PointertoNode POS;


struct NODE
{
    int value;
    struct NODE *next;
};

POS CreateList()
{
    POS temp;
    temp=(POS)malloc(sizeof(struct NODE));
      if(temp==NULL)
        printf("Out of Space\n");
      else
         {
          temp->value=-99;
          temp->next=NULL;
         }

    return temp;
}

POS FindFloor(int X,LIST L)
{
    POS P;
    P=L->next;

    if((P==NULL)||(X<P->value))
        return L;

      while((P!=NULL)&&(P->next!=NULL)&&((P->next->value)<X))
        P=P->next;

      return P;
}


void Insert(int X,LIST L,POS P)
{
    POS temp;
    temp=(POS)malloc(sizeof(struct NODE));
      if(temp==NULL)
        printf("Out of Space\n");
      else
      {
          temp->value=X;
          temp->next=P->next;
          P->next=temp;
      }
}




void PrintList(LIST L)
{
    POS P;
     P=L->next;

     printf("\nPRIORITY QUEUE\n-------\n");
     printf("#--->  ");

     while(P!=NULL)
     {
         printf("%d ",(P->value));
         P=(P->next);
     }

     printf("\n-------\n");
}

void PrintNode(POS P)
{
    printf("\n|-------------------|\n");
    printf("|  I D--  %10u|\n",P);
    printf("|-------------------|\n");
    printf("| VALUE-- %10d|\n",P->value);
    printf("| NEXT--- %10u|\n",P->next);
    printf("|-------------------|\n\n");

}

//#####################################################################################################




















void PrintMenu()
{
    printf("\n--------------------\n");
    printf("Press 1 to Queue\n");
    printf("Press 2 to Dequeue\n");
    printf("\nPress 0 to EXIT\n");
    printf("-----------------------\n");
}

int main()
{
    LIST L;

    L=CreateList();

   // printf("%u\n",L);

    int i;   int X,Y;
    POS P;   char C;

    do
    {
        system("cls");
        PrintList(L);
        PrintMenu();

        C=getch();

       switch(C)
       {

           case '1':  {

                        printf("\n Enter no. to Add\n");
                        scanf("%d",&X);

                        P=FindFloor(X,L);   // printf("\n%u",P);
                        Insert(X,L,P);

                      }  break;



            case '2':  {
                         P=L->next;
                         if(P!=NULL)
                         {
                            L->next=P->next;

                            printf("%d Removed!!!",P->value);
                           Sleep(500);
                           free(P);
                         }
                        }  break;






       }



    }while(C!='0');

   printf("\n------------BYE--------");
   Sleep(1000);


  return 0;
}






