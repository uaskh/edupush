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


POS Header (LIST L)
{
    return (L->next);
}

int IsEmpty(LIST L)
{
    return (L->next==NULL);
}

int IsLast(POS P,LIST L)
{
    return (P->next==NULL);
}

POS Find(int X,LIST L)
{
    POS P;
    P=L->next;
      while((P!=NULL)&&(P->value!=X))
        P=P->next;
      return P;
}

POS FindPrev(int X,LIST L)
{
    POS P;
    P=L;
      while((((P->next)->value)!=X)&&(P->next!=NULL))
        P=P->next;
    return P;
}



void Deletelist(LIST L)
{
    POS temp,P;
    temp=L->next;

      while(temp->next!=NULL)
      {
          P=temp;
          temp=temp->next;
          free(P);

      }
        free(temp);

        L->next=NULL;
}

void Delete(int X,LIST L)
{
    POS P,Q;
    P=Find(X,L);
      if(P!=NULL)
      {
          Q=FindPrev(X,L);

          Q->next=(P->next);
          free(P);
      }
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



POS Last(LIST L)
{
    while(L->next!=NULL)
        L=L->next;
    return L;
}

void Add(int X,LIST L)
{
   POS temp;
    temp=(POS)malloc(sizeof(struct NODE));
      if(temp==NULL)
        printf("Out of Space\n");
      else
      {
          temp->value=X;
          temp->next=NULL;
          Last(L)->next=temp;
      }
}


void PrintList(LIST L)
{
    POS P;
     P=L->next;

     printf("\nL I S T\n-------\n");
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
    printf("Press 1 to ADD\n");
    printf("Press 2 to Insert\n");
    printf("Press 3 to Find\n");
    printf("Press 4 to Delete\n");
    printf("Press 5 to Delete List\n");
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
           case '1': {
                      printf("\n Enter no. to Add\n");
                       scanf("%d",&X);
                       Add(X,L);     } break;

           case '2':  {
                       printf("\n Enter the number after you want to Insert\n");
                       scanf("%d",&Y);
                       P=Find(Y,L);
                       if(P!=NULL)
                       {
                        printf("\n Enter no. to Add\n");
                        scanf("%d",&X);
                        Insert(X,L,P);
                       }

                       else
                        {
                          printf("%d Not FOUND!!!",Y);
                          Sleep(400);
                        }
                   }  break;

           case '3':  {    printf("\n Enter no. to Find\n");
                       scanf("%d",&X);
                       P=Find(X,L);
                         if(P!=NULL)
                       {
                           PrintNode(P);
                           getche();

                       }

                       else
                          printf("%d Not FOUND!!!",X);

                        Sleep(500);
                   }  break;



            case '4':  {
                        printf("\n Enter no. to Delete\n");
                       scanf("%d",&X);
                       P=Find(X,L);
                         if(P!=NULL)
                       {
                           Delete(X,L);
                           printf("%d DELETED!!!",X);

                       }

                       else
                          printf("%d Not FOUND!!!",X);

                        Sleep(500);
                   }  break;



            case '5':
                      {
                          Deletelist(L);
                      } break;





       }



    }while(C!='0');

   printf("\n------------BYE--------");
   Sleep(1000);


  return 0;
}






