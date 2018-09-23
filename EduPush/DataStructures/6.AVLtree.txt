#include <stdio.h>
#include <stdlib.h>

#define MAX(a,b) (((a)>(b))?(a):(b))

struct AVLnode
{
    int value;
    struct AVLnode *Left;
    struct AVLnode *Right;
    int H;
};

typedef struct AVLnode *PtrtoAVLnode;

typedef PtrtoAVLnode Pos;
typedef PtrtoAVLnode AVLtree;


int Height(AVLtree P)
{
    if(P==NULL)
        return -1;

    else
        return(P->H);
}

void UpdateHeight(Pos K)
{
     K->H=MAX(Height(K->Left),Height(K->Right))+1;
}

int getBalance(Pos P)
{
    if (P==NULL)
        return 0;
    return Height(P->Left)-Height(P->Right);
}

AVLtree RotateRight(Pos K2)
{
    Pos K1;

    K1=K2->Left;
    K2->Left=K1->Right;
    K1->Right=K2;

   UpdateHeight(K2);

   UpdateHeight(K1);

   return K1;
}


AVLtree RotateLeft(Pos K2)
{
    Pos K1;

    K1=K2->Right;
    K2->Right=K1->Left;
    K1->Left=K2;

   UpdateHeight(K2);

   UpdateHeight(K1);

   return K1;


}

AVLtree DoubleRotateLR(Pos K3)
{
    K3->Left=RotateLeft(K3->Left);

    return(RotateRight(K3));
}

AVLtree DoubleRotateRL(Pos K3)
{
    K3->Right=RotateRight(K3->Right);

    return (RotateLeft(K3));
}


AVLtree AVLInsert(int X,AVLtree T)
{
    if(T==NULL)
    {

        T=(AVLtree)(malloc(sizeof(struct AVLnode)));

        if(T!=NULL)
        {
            T->value=X;
            T->Left=NULL;
            T->Right=NULL;
            T->H=0;


        }
    }



        else if(X<T->value)
        {
            T->Left=AVLInsert(X,T->Left);

            if(getBalance(T)>=2)
            {
                if(X<((T->Left)->value))
                    T=RotateRight(T);
                else
                    T=DoubleRotateLR(T);
            }
        }


        else if(X>T->value)
        {
            T->Right=AVLInsert(X,T->Right);

            if(getBalance(T)<=-2)
            {
                if(X>((T->Right)->value))
                    T=RotateLeft(T);
                else
                    T=DoubleRotateRL(T);
            }
        }



    UpdateHeight(T);

    return T;
}

void PrintNode(Pos P)
{
    printf("P--- %u\n left-- %u \n right-- %u \n height-- %d\n--------\n",P,P->Left,P->Right,P->H);
}

void PrintTree(AVLtree T)
{
    if(T!=NULL)
    {
    printf("%d ",T->value);
    PrintTree(T->Left);
    PrintTree(T->Right);
    }
}





Pos minValueNode(AVLtree T)
{
   while(T->Left!=NULL)
    T=T->Left;
  return T;
}











AVLtree Delete(int X,AVLtree T)
{
    Pos temp;

     if(T==NULL)
     return T;

     if(X<T->value)
     T->Left=Delete(X,T->Left);

    else if(X>T->value)
    T->Right=Delete(X,T->Right);

   else
    {
         if((T->Left==NULL)||(T->Right==NULL))
         {
            temp=T->Left?T->Left:T->Right;

            if(temp==NULL)
            {
                temp=T;
                T=NULL;
             }

             else
              {
                 *T=*temp;
               }

             free(temp);
          }



        else
           {
              temp=minValueNode(T->Right);

              T->value=temp->value;

               T->Right=Delete(temp->value,T->Right);
            }


      }


       if(T==NULL)
      return NULL;


    UpdateHeight(T);




           if(getBalance(T)>=2)
            {
                if(getBalance(T->Left)>=1)
                    T=RotateRight(T);
                else
                    T=DoubleRotateLR(T);
            }


            else if(getBalance(T)<=-2)
            {
                if(getBalance(T->Right)<=-1)
                    T=RotateLeft(T);
                else
                    T=DoubleRotateRL(T);
            }

    return T;

}



int main()
{
    int num;
    int i;
    int N;
    scanf("%d",&N);

    AVLtree TR;

    TR=NULL;
    int choice;

    printf("TR--%u  \n",TR);

    for(i=1;i<=N;i++)
    {
        scanf("%d",&choice);


        scanf("%d",&num);

        if(choice==1)
        TR=AVLInsert(num,TR);

        else
        TR=Delete(num,TR);

         printf("\n--------------------\n");
        printf("TR--%u  Height-- %d\n",TR,TR->H);
        PrintTree(TR);
        printf("\n--------------------\n");
    }

    return 0;
}
