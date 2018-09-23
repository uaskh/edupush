#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10

typedef struct  Node *PointertoNode;
typedef PointertoNode NODE;


struct Node
{
    int value;
    NODE next;
};


struct QueueData
{
    NODE front;
    NODE rear;
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
        temp->front=NULL;
        temp->rear=NULL;
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
    return ((Q->front)->value);

    else
        return(-999);
}

void Dequeue(QUEUE Q)
{
    if(!IsEmpty(Q))
      {
          NODE temp;
          temp=Q->front;
          Q->front=temp->next;

          free(temp);

            Q->size--;

            
      }
}

void Enqueue(int X,QUEUE Q)
{
    if(!IsFull(Q))
      {

          NODE temp;
          temp=(NODE)malloc(sizeof(struct Node));

          if(temp==NULL)
            printf("ERROR Memory Full\n");

          else
          {
              temp->value=X;
              temp->next=NULL;

              if(IsEmpty(Q))
              {
                  Q->front=temp;
                  Q->rear=temp;
                  Q->size++;
              }

              else
              {
                  (Q->rear)->next=temp;
                  Q->rear=temp;
                  Q->size++;
              }

          }
      }

}

void MakeEmpty(QUEUE Q)
{
    while(!IsEmpty(Q))
        Dequeue(Q);
}

void PrintQueue(QUEUE Q)
{
    int i;  i=Q->size;

    NODE temp=Q->front;

     printf("-------------------\n");
    while(i--)
    {
       printf("%d ",((temp)->value));
       temp=temp->next;

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

        printf("Front=%u\n",Q->front);
        printf("Rear=%u\n",Q->rear);
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
