#include <stdio.h>
#include <stdlib.h>
#include <windows.h>

struct LHNode
{
    int value;
    struct LHNode *left;
    struct LHNode *right;
    int npl;
};

typedef struct LHNode *PointertoNode;
typedef PointertoNode LH;
typedef PointertoNode POS;


    LH merge(LH,LH);
    LH Fun(LH,LH);


LH MakeLHNode(int X)
{
    POS P;
    P=malloc(sizeof(struct LHNode));

    P->value=X;
    P->left=NULL;
    P->right=NULL;
    P->npl=0;
}

void SwapChildren(POS P)
{
    POS temp;

    temp=P->left;
    P->left=P->right;
    P->right=temp;
}

LH merge(LH H1,LH H2)
{
    if(H1==NULL)
        return H2;
    if(H2==NULL)
        return H1;

     if(H1->value<H2->value)
        return Fun(H1,H2);

     else
        return Fun(H2,H1);
}

LH Fun(LH H1,LH H2)
{
    if(H1->left==NULL)
        H1->left=H2;

    else
    {
        H1->right=merge(H1->right,H2);

        if((H1->left->npl)<(H1->right->npl))
            SwapChildren(H1);

        H1->npl=(H1->right->npl)+1;
    }

    return(H1);
}


LH Insert(int X,LH H)
{
    POS P;
    P=MakeLHNode(X);

    if(H==NULL)
        return P;

    H=merge(P,H);

    return H;
}


LH Deletemin(LH H)
{
    LH L,R;

    if(H!=NULL)
    {
        L=H->left;
        R=H->right;
        free(H);
        return(merge(L,R));
    }
}
void printLH(LH H)
{
    if(H!=NULL)
    {

    printf("value-- %d  npl--%d  ID--%u  left--%u  right--%u\n",H->value,H->npl,H,H->left,H->right);

    printLH(H->left);
    printLH(H->right);

    }
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


    LH LFH;

  LFH=NULL;
  
    int i;   int X,Y;
    char C;

    do
    {
        system("cls");
        printLH(LFH);
        PrintMenu();

        C=getch();

       switch(C)
       {

           case '1':  {

                        printf("\n Enter no. to insert\n");
                        scanf("%d",&X);
                        LFH=Insert(X,LFH);

                      }  break;



            case '2':  {

                        if(LFH!=NULL)
                         {
                             LFH=Deletemin(LFH);
                            //printf("%d Removed!!!",RemoveTop(PQ));
                           Sleep(500);
                         }
                        }  break;


       }



    }while(C!='0');

   printf("\n------------BYE--------");
   Sleep(1000);


  return 0;
}
