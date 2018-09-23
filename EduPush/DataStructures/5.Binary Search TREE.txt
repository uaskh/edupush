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
typedef PtrtoTree BST;


BST CreateTree()
{
    BST T;
    T=(BST)(malloc(sizeof(struct TreeData)));
      if(T!=NULL)
      {
          T->Root=NULL;
          T->size=0;
      }

      return (T);
}

void Add(int X,POS P)
{
    if(X<P->value)
    {
        if(P->left==NULL)
        {
                            TNODE temp=(TNODE)malloc(sizeof(struct TreeNode));
                            if(temp!=NULL)
                            {
                            temp->left=NULL;
                            temp->right=NULL;
                            temp->parent=P;
                            temp->S='L';
                            temp->value=X;
                            }

                        P->left=temp;
             }

             else
              Add(X,P->left);
    }




    else if(X>P->value)
    {
        if(P->right==NULL)
        {
                            TNODE temp=(TNODE)malloc(sizeof(struct TreeNode));
                            if(temp!=NULL)
                            {
                            temp->left=NULL;
                            temp->right=NULL;
                            temp->parent=P;
                            temp->S='R';
                            temp->value=X;
                            }

                        P->right=temp;
             }

             else
              Add(X,P->right);
    }

}

POS Find(int X,TNODE T)
{
    if(T==NULL)
        return NULL;

    if(X<T->value)
        return  Find(X,T->left);

    else if(X>T->value)
        return Find(X,T->right);

    else
        return T;
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

void PrintTree(BST T)
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


void Cut(POS P,BST T)
{
    if(P!=T->Root)
    {
    if(P->S=='L')
        P->parent->left=NULL;
    else
        P->parent->right=NULL;
    }

    else
        T->Root=NULL;

    MakeEmpty(P);

}


POS FindMin(POS P)
{
    if(P==NULL)
        return NULL;

    else if(P->left==NULL)
        return P;

    else
        return FindMin(P->left);
}


POS FindMax(POS P)
{
    if(P==NULL)
        return NULL;

    else if(P->right==NULL)
        return P;

    else
        return FindMax(P->right);
}





//########################################################################################################
































void PrintMenu()
{
    printf("\n--------------------\n");
    printf("Press 1 to Add\n");
    printf("Press 2 to Find\n");
    printf("Press 3 to Find Min\n");
    printf("Press 4 to Find Max\n");
    printf("Press 5 to Cut Branch\n");
    printf("Press 6 to Make Empty\n");
    printf("\nPress 0 to EXIT\n");
    printf("-----------------------\n");
}


int main()
{
    BST T;

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
                             Add(X,T->Root);
                          }
                        } break;

           case '2':  {

                        printf("\n Enter no. to Find\n");
                        scanf("%d",&X);
                        POS P=Find(X,T->Root);

                        if(P!=NULL)
                        {
                            printf("Yess  %u\n",P);    getchar();
                        }

                       }break;

           case '3':  {
                         printf("Min=%d",(FindMin(T->Root))->value);   getche();
                              }  break;

            case '4':  {
                         printf("Max=%d",(FindMax(T->Root))->value);    getche();
                              }  break;



           case '5':  {
                            printf("Enter the number you wanna cut..\n");
                            scanf("%d",&X);

                             POS P=Find(X,T->Root);

                        if(P!=NULL)
                        {
                            Cut(P,T);
                        }

                       }  break;


           case '6': { MakeEmpty(T->Root);
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
