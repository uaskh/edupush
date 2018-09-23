#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10


struct QueueData
{
    int Que[MAXSIZE];
    int front;
    int rear;
    int size;
};

typedef struct QueueData *PointertoQueue;
typedef PointertoQueue QUEUE;

QUEUE CreateQueue()
{
    QUEUE temp;

    temp=(QUEUE)malloc(sizeof(struct QueueData));

    if(temp==NULL)
        printf("ERROR...Out of memory\n");

    else
    {
        temp->front=0;
        temp->rear=-1;
        temp->size=0;
    }

    return temp;

}


int IsEmpty(QUEUE Q)
{
    return (Q->size==0);
}

int IsFull(QUEUE Q)
{
    return (Q->size==MAXSIZE);
}

int Front(QUEUE Q)
{
    if(!IsEmpty(Q))
    return (Q->Que[Q->front]);

    else
        return(-999);
}

void Dequeue(QUEUE Q)
{
    if(!IsEmpty(Q))
      {    if(Q->front==MAXSIZE-1)
            Q->front=0;
            else
            (Q->front)++;

            Q->size--;
      }
}

void Enqueue(int X,QUEUE Q)
{
    if(!IsFull(Q))
      {    if(Q->rear==MAXSIZE-1)
            Q->rear=0;
            else
            (Q->rear)++;

          Q->Que[Q->rear]=X;

            Q->size++;
      }

}

void MakeEmpty(QUEUE Q)
{
    Q->front=0;
    Q->rear=-1;
    Q->size=0;
}

void PrintQueue(QUEUE Q)
{
    int i,j;   j=Q->front;

     printf("-------------------\n");
    for(i=Q->size;i>0;i--)
    {
       printf("%d ",Q->Que[j]);

       if(j!=MAXSIZE-1)
        j++;

       else
        j=0;
    }

    printf("\n-------------------\n");
}
































//####################################################################################################################3


void PrintMenu()
{
    printf("\n--------------------\n");
    printf("Press 1 to see FRONT\n");
    printf("Press 2 to ENQUEUE\n");
    printf("Press 3 to DEQUEUE\n");
    printf("Press 4 to Empty Queue\n");
    printf("\nPress 0 to EXIT\n");
    printf("-----------------------\n");
}



int main()
{
    QUEUE Q;

    Q=CreateQueue();

   // printf("%u\n",L);

    int i;   int X,Y;
    char C;

    do
    {
        system("cls");

        printf("Front=%d\n",Q->front);
        printf("Rear=%d\n",Q->rear);
        printf("Size=%d\n",Q->size);

        PrintQueue(Q);
        PrintMenu();

        C=getch();

       switch(C)
       {
           case '1': {
                         X=Front(Q);
                         printf("On Front== %d\n",X);
                        } break;

           case '2':  {

                        printf("\n Enter no. to Enqueue\n");
                        scanf("%d",&X);
                        Enqueue(X,Q);
                       }break;

           case '3':  {
                         X=Front(Q);

                         Dequeue(Q);
                         system("cls");
                        // PrintQueue(Q);

                          printf("%d Dequeued\n",X);

                         // getche();

                       }  break;


           case '4':  MakeEmpty(Q);  break;


            case '0':  {

                          printf("BYEEEEEEE\n");
                          exit(0);
                       }

       }

    }while(C!='0');

    return 0;

}
