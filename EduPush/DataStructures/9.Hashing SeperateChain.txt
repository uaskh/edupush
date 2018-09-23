#include <stdio.h>
#include <stdlib.h>


    struct Node
    {
        int value;
        struct Node *next;
    };

    struct HashTable
    {
        int size;
        struct Node *LIST[100];
    };


    typedef struct Node *PointertoNode;
    typedef PointertoNode POS;

    typedef struct HashTable *PointertoHashTable;
    typedef PointertoHashTable HASHTABLE;



int Hash(int n,int s)
{
    return n%s;
}

HASHTABLE makeHT(int n)
{
   int i;
   HASHTABLE HT=malloc(sizeof(struct HashTable));

   HT->size=n;

   for(i=0;i<HT->size;i++)
        (HT->LIST)[i]=NULL;

        return HT;
}

void printHT(HASHTABLE HT)
{
    int i,j;
    int s=HT->size;  POS P;

    for(i=0;i<s;i++)
    {
        P=(HT->LIST)[i];

        while(P!=NULL)
        {
            printf("%d ",P->value);
            P=P->next;
        }

        printf("NULL\n");
    }
}

void insert(POS P,HASHTABLE HT,int k)
{
    POS Q;  int t;  t=0;

    if((HT->LIST)[k]==NULL)
        (HT->LIST)[k]=P;

       // P->next=NULL;

    else
    {
        Q=(HT->LIST)[k];

        if((Q->value)>(P->value))
         {
             (HT->LIST)[k]=P;
             P->next=Q;
         }


        while(Q->next!=NULL)
        {
            if((Q->next->value)>(P->value))
            {
                P->next=Q->next;
                Q->next=P;
                t=1;
                break;
            }
            Q=Q->next;
        }

        if(t!=1)
        {
           Q->next=P;
           P->next=NULL;
        }

    }
}


int main()
{

    HASHTABLE HST;
     int X,K,S,i;
    POS P;

    HST=makeHT(10);

   // printHT(HST);

    int Total;
    scanf("%d",&Total);

    S=(HST->size);


    while(Total--)
    {
        printHT(HST);

        scanf("%d",&X);

        K=Hash(X,S);

        P=malloc(sizeof(struct Node));

        P->value=X;
        P->next=NULL;

        insert(P,HST,K);

        system("cls");
    }


    return 0;
}
