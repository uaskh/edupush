#include <stdio.h>
#include <stdlib.h>

struct Node
{
    int key;  int order;
    struct Node *child;
    struct Node *sibling;
    struct Node *parent;
};


typedef struct Node *PointertoNode;
typedef PointertoNode BTree;
typedef PointertoNode POS;
typedef PointertoNode BHeap;
typedef PointertoNode NODE;

NODE newNode(int x)
{
    POS P;
    P=malloc(sizeof(struct Node));
    P->key=x;  P->order=0;
    P->child=NULL;
    P->parent=NULL;
    P->sibling=NULL;

    return P;
}

POS lastChild(POS P)
{
    POS Q;

    Q=P->child;

      if(Q==NULL)
        return NULL;

    while((Q->sibling)!=NULL)
    {
        Q=Q->sibling;
    }

    return Q;
}


void MakeChild(BTree B1,BTree B2)
{
    POS R;

        R=lastChild(B1);

        if(R==NULL)
            B1->child=B2;

        else
            R->sibling=B2;

        B2->parent=B1;
        B2->sibling=NULL;

        (B1->order)++;

}


POS Fun(POS X)
{
    POS next;  POS prev;
                prev=NULL;

    POS start;

    start=X;

    while(X!=NULL)
    {
        next=X->sibling;

        if(next==NULL)
         break;

        if((X->order)!=(next->order))
        {
            prev=X;
            X=X->sibling;
            continue;
        }

        else
        {
            if((next->sibling!=NULL)&&((next->order)==(next->sibling->order)))
            {
                prev=X;
                X=X->sibling;
                continue;
            }

            else
            {
                if((X->key)<=(next->key))
                {
                    X->sibling=next->sibling;
                    MakeChild(X,next);
                }

                else
                {
                    if(prev!=NULL)
                    prev->sibling=next;

                    else
                    start=next;

                   MakeChild(next,X);

                   X=next;
                }

            }
        }
    }

    return start;
}


void PrintNode(POS P)
{
    if(P!=NULL)
    {
       printf("%d ",P->key);
       PrintNode(P->sibling);
       PrintNode(P->child);
    }
}

void PrintBT(POS Q)
{
    if(Q!=NULL)
    {
        printf("%d ",Q->key);
        PrintNode(Q->child);
    }
    printf("\n");
}
void PrintBH(POS P)
{
   while(P!=NULL)
   {
       PrintBT(P);
       printf("\n\n\n");
       P=P->sibling;
   }
}

int main()
{
    BHeap BH;
    BH=NULL;

    POS P;
    int x;

    do
    {
        printf("\n---------------------------------------------------------\n");
        printf("Enter a number to add\n");
        scanf("%d",&x);

        P=newNode(x);

        P->sibling=BH;
        BH=P;

        BH=Fun(BH);

        PrintBH(BH);

    }while(x!=-99);

    return 0;
}
