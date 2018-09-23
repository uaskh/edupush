#include <stdio.h>
#include <stdlib.h>

struct TreeNode
{
    int value;
    struct TreeNode *left;
    struct TreeNode *right;
    struct TreeNode *parent;
    char S;
};

typedef struct TreeNode *PtrtoNode;
typedef PtrtoNode TNODE;
typedef PtrtoNode POS;

struct TreeData
{
   TNODE Root;
   int size;
   int height;
};

typedef struct TreeData *PtrtoTree;
typedef PtrtoTree TREE;


TREE CreateTree()
{
    TREE T;
    T=(TREE)(malloc(sizeof(struct TreeData)));
      if(T!=NULL)
      {
          T->Root=NULL;
          T->size=0;
      }

      return (T);
}


void AddLeft(int X,POS P,TREE T)
{
    if(P->left==NULL)
    {
        TNODE temp=(TNODE)(malloc(sizeof(struct TreeNode)));
        temp->left=NULL;
        temp->right=NULL;
        temp->parent=P;
        temp->value=X;
        temp->S='L';

        P->left=temp;
    }
}


void AddRight(int X,POS P,TREE T)
{
    if(P->right==NULL)
    {
        TNODE temp=(TNODE)(malloc(sizeof(struct TreeNode)));
        temp->left=NULL;
        temp->right=NULL;
        temp->parent=P;
        temp->value=X;
        temp->S='R';

        P->right=temp;
    }
}


POS Find(int X,TNODE T)
{
    TNODE L,R;
    if(T!=NULL)
    {
        if(T->value==X)
            return(T);

        L=Find(X,T->left);
          if(L!=NULL)
            return(L);

       R=Find(X,T->right);
       return(R);
    }

    else
        return(NULL);
}

void PrintNodes(POS P)
{
    if(P!=NULL)
    {
    printf("%d ",P->value);
    PrintNodes(P->left);
    PrintNodes(P->right);
    }
}

void PrintTree(TREE T)
{
    PrintNodes(T->Root);
}


void MakeEmpty(POS P)
{
    if(P!=NULL)
    {
        MakeEmpty(P->left);
        MakeEmpty(P->right);
        free(P);
    }
}


void Cut(POS P,TREE T)
{
    if(P->S=='L')
        P->parent->left=NULL;
    else
        P->parent->right=NULL;

    MakeEmpty(P);

}


//########################################################################################################
































void PrintMenu()
{
    printf("\n--------------------\n");
    printf("Press 1 to Add\n");
    printf("Press 2 to Find\n");
    printf("Press 3 to Cut Branch\n");
    printf("Press 4 to Make Empty\n");
    printf("\nPress 0 to EXIT\n");
    printf("-----------------------\n");
}


int main()
{
    TREE T;

    T=CreateTree();

   // printf("%u\n",L);

    int i;   int X,Y;
    char C,S;

    do
    {
        system("cls");


        printf("Size=%d\n",T->size);

        PrintTree(T);
        PrintMenu();

        C=getch();

       switch(C)
       {
           case '1': {
                        printf("\nEnter number to Add\n");
                            scanf("%d",&X);

                        if(T->Root==NULL)
                        {
                            TNODE temp=(TNODE)malloc(sizeof(struct TreeNode));
                            if(temp!=NULL)
                            {
                            temp->left=NULL;
                            temp->right=NULL;
                            temp->parent=NULL;
                            temp->value=X;
                            }

                            T->Root=temp;

                          }

                          else
                          {
                              printf("Enter the number after which you wanna add..\n");
                              scanf("%d",&Y);

                              POS P=Find(Y,T->Root);
                              if(P!=NULL)
                              {
                                  printf("Press L to add in left\nPress R to add in Right\n");
                                  S=getche();

                                  if(S=='L')
                                    AddLeft(X,P,T);

                                  else if(S=='R')
                                    AddRight(X,P,T);
                              }
                          }
                        } break;

           case '2':  {

                        printf("\n Enter no. to Find\n");
                        scanf("%d",&X);
                        POS P=Find(X,T->Root);

                        if(P!=NULL)
                        {
                            printf("Yess  %u\n",P);
                        }

                       }break;

           case '3':  {
                            printf("Enter the number you wanna cut..\n");
                            scanf("%d",&X);

                             POS P=Find(X,T->Root);

                        if(P!=NULL)
                        {
                            Cut(P,T);
                        }

                       }  break;


           case '4': { MakeEmpty(T->Root);
                        T->Root=NULL;
                        T->size=0;
                        } break;


            case '0':  {

                          printf("BYEEEEEEE\n");
                          exit(0);
                       }

       }

    }while(C!='0');

    return 0;

}
