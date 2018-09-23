#include <stdio.h>
#include <stdlib.h>
#include <windows.h>

struct HeapData
{
    int capacity;
    int size;
    int *value;
};

typedef struct HeapData *PointertoHeap;
typedef PointertoHeap PRIORITYQUEUE;

int isFull(PRIORITYQUEUE PQ)
{
    if(PQ->size==PQ->capacity)
        return 1;
    else
        return 0;
}


int isEmpty(PRIORITYQUEUE PQ)
{
    if(PQ->size==0)
        return 1;

    else
        return 0;
}

PRIORITYQUEUE MakePQ(int n)
{
    PRIORITYQUEUE PQ;

    PQ=malloc(sizeof(struct HeapData));

    PQ->value=malloc((sizeof(int))*(n+1));
    PQ->size=0;
    PQ->capacity=n;
    (PQ->value)[1]=-999;

    return PQ;

}

void Insert(int X,PRIORITYQUEUE PQ)
{
    int i;

    if(isFull(PQ))
        printf("Error!!! priority queue FULL\n");

    else if(isEmpty(PQ))
    {    
        PQ->value[1]=X;
        PQ->size++;
    }

    else
    {
        i=++(PQ->size);

        while(((PQ->value[i/2])>X)&&(i>1))
        {
            PQ->value[i]=PQ->value[i/2];
            i=i/2;
        }

        PQ->value[i]=X;

    }

}



int RemoveTop(PRIORITYQUEUE PQ)
{
    int i,C;
    int min,last;

    if(isEmpty(PQ))
    {
        printf("Error!! Priority Queue Empty\n");
        return -999;
    }

    min=PQ->value[1];
    last=PQ->value[PQ->size];

    for(i=1;(2*i<=PQ->size);i=C)
    {
        C=2*i;

        if((C!=PQ->size)&&(PQ->value[C]>PQ->value[C+1]))
        C=C+1;

        if(last>(PQ->value[C]))
            PQ->value[i]=PQ->value[C];
        else
            break;
    }

    PQ->value[i]=last;  PQ->size--;
    return(min);

}

void printPQ(PRIORITYQUEUE PQ)
{
    int i;
    for(i=1;i<=PQ->size;i++)
        printf("%d ",(PQ->value)[i]);
    printf("\n---------------------------\n");
}

void PrintMenu()
{
    printf("\n--------------------\n");
    printf("Press 1 to Insert\n");
    printf("Press 2 to PopTop\n");
    printf("\nPress 0 to EXIT\n");
    printf("-----------------------\n");
}



int main()
{
    int M;
    printf("Enter the capacity of Priority Queue\n");
    scanf("%d",&M);

    PRIORITYQUEUE PQ;
    PQ=MakePQ(M);

   // printf("%u\n",L);

    int i;   int X,Y;
    char C;

    do
    {
        system("cls");
        printPQ(PQ);
        PrintMenu();

        C=getch();

       switch(C)
       {

           case '1':  {

                        printf("\n Enter no. to insert\n");
                        scanf("%d",&X);
                        Insert(X,PQ);

                      }  break;



            case '2':  {

                         if(!isEmpty(PQ))
                         {
                            printf("%d Removed!!!",RemoveTop(PQ));
                           Sleep(500);
                         }
                        }  break;


       }



    }while(C!='0');

   printf("\n------------BYE--------");
   Sleep(1000);


  return 0;
}
