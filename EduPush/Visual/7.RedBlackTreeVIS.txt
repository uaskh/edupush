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




//------------------------------------------------------------------------------------------------------------------------


void Fill(int TA[],int CR[],int i,POS P)
{
    if(P!=NULL)
    {

      TA[i]=P->value;
      CR[i]=P->color;
      Fill(TA,CR,2*i+1,P->left);
      Fill(TA,CR,2*i+2,P->right);

    }

}


void UpdateTaree(int TA[],int CR[],RBTree T)
{   int i;
    for(i=0;i<100;i++)
        {  TA[i]=-99;  CR[i]=-99;  }
    Fill(TA,CR,0,T);
}


int MAX=130;
int CTR=70;

void FillCN(int Crr[],int X,int i,int p)
{
    int Pr;
    if(p>=0)
    {
    Crr[i]=X;
    Pr=pow(2,p);
    FillCN(Crr,X-Pr,2*i+1,p-1);
    FillCN(Crr,X+Pr,2*i+2,p-1);
    }
}

void MakeCN(int Crr[],int X,int h)
{
    int i;
    for(i=0;i<100;i++)
        Crr[i]=0;
    FillCN(Crr,X,0,h);
}




void DrawTree(int TAR[],int CR[],int CN[],int H)
{
    int l,h,e,c,k,i;
    h=H; e=1;

    printf("\n\n\n");
    for(i=0;i<MAX;i++)
    {
        if(i==CTR)
            printf("%d",TAR[0]);
        else
            printf(" ");
    }

    printf("\n");


    while(h--)
    {
        l=(int)pow(2,e)-1;
          k=(int)pow(2,h)-1;

      if(k==0)
        break;

        for(;k>=0;k--)
        {
            c=l;

            for(i=0;i<=MAX;i++)
            {
                if((c%2==0)&&(i==CN[c]-k))
                {
                    if(TAR[c]!=-99)
                    {
                        if(k==0)
                           {
                               printf("%d",TAR[c]);

                               if(TAR[c]>9||TAR[c]<0)  i++;
                           }
                        else if(k==1)
                        {
                            if(CR[c]==1)
                            printf("R");

                            else
                                printf("B");


                        }

                        else
                            printf("\\");
                    }
                    else
                        printf(" ");
                    c++;
                }

                else if((c%2!=0)&&(i==CN[c]+k))
                {
                    if(TAR[c]!=-99)
                    {
                        if(k==0)
                           {
                               printf("%d",TAR[c]);

                               if(TAR[c]>9||TAR[c]<0)  i++;
                           }
                        else if(k==1)
                        {
                            if(CR[c]==1)
                            printf("R");

                            else
                                printf("B");


                        }

                        else
                            printf("/");
                    }
                    else
                        printf(" ");
                    c++;
                }

                else
                    printf(" ");
            }

            printf("\n");
        }

        e++;
    }

}




int main()
{
   int n,X; int i,j;

   RBTree RBT;
   RBT=NULL;

   int Tarre[100];  int Color[100];  int HT;
   int CN[100];

   scanf("%d",&n);

   for(i=0;i<100;i++)
     {
         Tarre[i]=-99;
         Color[i]=-99;
     }


   for(i=1;i<=n;i++)
   {

       HT=(int)log2(i)+1;

       scanf("%d",&X);

       InsertRBT(X,RBT,&RBT);

      // printRBTree(RBT);

       UpdateTaree(Tarre,Color,RBT);
       MakeCN(CN,CTR,HT);


      // for(j=0;j<20;j++)
       // printf("value - %d color - %d\n",Tarre[j],Color[j]);


       DrawTree(Tarre,Color,CN,HT+1);

   }
    return 0;
}
