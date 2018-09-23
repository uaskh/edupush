#include <stdio.h>
#include <stdlib.h>


struct SPTNode
{
    int value;
    struct SPTNode *left;
    struct SPTNode *right;
    struct SPTNode *parent;
};

typedef struct SPTNode *PointertoNode;
typedef PointertoNode SPTree;
typedef PointertoNode ROOT;
typedef PointertoNode POS;

void RotateLeft(POS P,SPTree *RP)
{

    POS T,Q;

    Q=P->right;

    if(P->parent!=NULL)
     {
         if(P==(P->parent->right))
            P->parent->right=Q;
         else
            P->parent->left=Q;

            Q->parent=P->parent;
     }

     else
     {
         *RP=Q;  Q->parent=NULL;
     }

    T=Q->left;   if(T!=NULL) T->parent=P;
    P->right=T;
    Q->left=P;
    P->parent=Q;

}

void RotateRight(POS P,SPTree *RP)
{

    POS T,Q;

    Q=P->left;

    if(P->parent!=NULL)
     {
         if(P==(P->parent->left))
            P->parent->left=Q;
         else
            P->parent->right=Q;

            Q->parent=P->parent;
     }

     else
     {
         *RP=Q;  Q->parent=NULL;
     }

    T=Q->right;   if(T!=NULL) T->parent=P;
    P->left=T;
    Q->right=P;
    P->parent=Q;

}

void Splay(POS K,SPTree *RP)
{

    ROOT RT;

    POS PT,GP;

    while(K!=RT)
    {
        RT=*RP;

        PT=K->parent;

        if(PT==RT)
        {
            if(K==RT->left)
                RotateRight(PT,RP);
            else
                RotateLeft(PT,RP);

                 break;


        }


        else
        {
            GP=PT->parent;
                             // printf("K- %u PT-%u  GP-%u\n",K,PT,GP);

            if(PT==GP->left)
            {
                if(K==PT->left)
                {
                    RotateRight(GP,RP);
                    RotateRight(PT,RP);
                }

                else
                {
                    RotateLeft(PT,RP);
                    RotateRight(GP,RP);
                }
            }

            else if(PT==GP->right)
            {
                if(K==PT->right)
                {
                    RotateLeft(GP,RP);
                    RotateLeft(PT,RP);
                }

                else
                {
                    RotateRight(PT,RP);
                    RotateLeft(GP,RP);
                }
            }




        }
    }

    K->parent=NULL;
    *RP=K;

}

POS NewNode(int X)
{
    POS N=malloc(sizeof(struct SPTNode));

    N->value=X;
    N->left=NULL;
    N->right=NULL;
    N->parent=NULL;

    return N;
}


void SPTInsert(int X,POS P,SPTree *RP)
{
    POS N;

    if(*RP==NULL)
    {
      N=NewNode(X);
      *RP=N;
    }

    else
    {
        if(X<P->value)
        {
            if(P->left!=NULL)
                SPTInsert(X,P->left,RP);

            else
            {
                N=NewNode(X);
                N->parent=P;
                P->left=N;    // printSPTree(*RP);  printf("\n---------------------------------------\n");

                Splay(N,RP);
            }
        }

        else
        {
            if(P->right!=NULL)
                SPTInsert(X,P->right,RP);

            else
            {
                N=NewNode(X);
                N->parent=P;
                P->right=N;   //printSPTree(*RP);  printf("\n---------------------------------------\n");
                Splay(N,RP);

            }
        }
    }
}



void printSPTree(SPTree P)
{

  if(P!=NULL)
  {

    printf("value-- %d  K-%u  PT-%u  Left-%u  Right-%u \n",P->value,P,P->parent,P->left,(P->right));

    printSPTree(P->left);
    printSPTree(P->right);
  }

}







int main()
{

  SPTree SPT;
  SPT=NULL;

  int n,X,i;
  scanf("%d",&n);

  for(i=0;i<n;i++)
  {
      scanf("%d",&X);

      SPTInsert(X,SPT,&SPT);


      printf("\n--------------------------\n");
      printf("Root- %u \n",SPT);
      printSPTree(SPT);

     printf("\n---------------------------------------\n");
  }

    return 0;
}
