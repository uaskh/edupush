#include <stdio.h>
#include <stdlib.h>

#define RED 1
#define BLACK 0

struct RBTNode
{
    int value;

    struct RBTNode *parent;
    struct RBTNode *left;
    struct RBTNode *right;

    int color;
};


typedef struct RBTNode *PointertoNode;
typedef PointertoNode RBTree;
typedef PointertoNode POS;


void SwapColor(POS A,POS B)
{
    int temp=(A)->color;

    (A)->color=(B)->color;
    (B)->color=temp;
}


void RotateRight(POS P,RBTree *RP)
{
    POS Q;

    if(P==*RP)
    {
        Q=P->left;

        P->left=Q->right;
        P->parent=Q;

        Q->right=P;
        Q->parent=NULL;

        *RP=Q;
    }

    else
    {
        Q=P->left;

        if(P==(P->parent)->left)
            (P->parent)->left=Q;
        else
            (P->parent)->right=Q;

        Q->parent=P->parent;

        P->left=Q->right;
        P->parent=Q;

        Q->right=P;

    }
}


void RotateLeft(POS P,RBTree *RP)
{
    POS Q;

    if(P==*RP)
    {
        Q=P->right;

        P->right=Q->left;
        P->parent=Q;

        Q->left=P;
        Q->parent=NULL;

        *RP=Q;
    }

    else
    {
        Q=P->right;

        if(P==(P->parent)->right)
            (P->parent)->right=Q;
        else
            (P->parent)->left=Q;

        Q->parent=P->parent;

        P->right=Q->left;
        P->parent=Q;

        Q->left=P;

    }
}





RBTree Fixation(RBTree root,POS K)
{

    POS PT;
    POS GP;
    POS UC;

    PT=K->parent;

    while(K!=NULL&&PT!=NULL&&K->color==RED&&PT->color==RED)
    {
       GP=PT->parent;


       if(PT==GP->left)
       {
           UC=GP->right;

           if(UC!=NULL&&UC->color==RED)
           {
               GP->color=RED;
               UC->color=BLACK;
               PT->color=BLACK;

               K=GP;
           }

           else
           {

               if(K==PT->right)
               {
                 RotateLeft(PT,&root);
                 SwapColor(GP,K);
               }

               else
               SwapColor(GP,PT);

               RotateRight(GP,&root);

           }

           K=K->parent;

       }


       else if(PT==GP->right)
       {

           UC=GP->left;

           if(UC!=NULL&&UC->color==RED)
           {
               GP->color=RED;
               UC->color=BLACK;
               PT->color=BLACK;

               K=GP;
           }

           else
           {
               if(K==PT->left)
              {
                RotateRight(PT,&root);
                SwapColor(GP,K);
              }

              else
               SwapColor(GP,PT);

               RotateLeft(GP,&root);


           }

           K=K->parent;
       }




    }

    root->color=BLACK;
    root->parent=NULL;

    return root;

}

POS NewNode(int X)
{
    POS N=malloc(sizeof(struct RBTNode));

           N->value=X;
           N->color=RED;
           N->left=NULL;
           N->right=NULL;

    return N;
}


void InsertRBT(int X,POS P,POS *RP)
{
    POS N;

    if(*RP==NULL)
    {
        N=NewNode(X);
        N->color=BLACK;
        N->parent=NULL;
        *RP=N;

    }

    else
    {

    if(X<P->value)
    {
        if(P->left!=NULL)
          InsertRBT(X,P->left,RP);

        else
        {
            N=NewNode(X);

            N->parent=P;
            P->left=N;

            *RP=Fixation(*RP,N);


        }
    }

    else
    {
        if(P->right!=NULL)
          InsertRBT(X,P->right,RP);

        else
        {
            N=NewNode(X);

            N->parent=P;
            P->right=N;

            *RP=Fixation(*RP,N);

        }
    }

   }
}



void printRBTree(RBTree P)
{

  if(P!=NULL)
  {

    if(P->color==1)
        printf(" -- R--");
    else
        printf(" -- B--");

    printf("%d  P-%u  PT-%u  Left-%u  Right-%u \n",P->value,P,P->parent,P->left,(P->right));

    printRBTree(P->left);
    printRBTree(P->right);
  }
}







int main()
{
   int n,X; int i;

   RBTree RBT;
   RBT=NULL;

   scanf("%d",&n);

   for(i=1;i<=n;i++)
   {
       scanf("%d",&X);

       InsertRBT(X,RBT,&RBT);

       printRBTree(RBT);
   }
    return 0;
}
