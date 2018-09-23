#include <stdio.h>
#include <stdlib.h>


struct node
{
    int value;
    struct node *next;
};

typedef struct node *PointertoNode;
typedef PointertoNode NODE;

struct nodestart
{
    struct node *head;
};

typedef struct nodestart *PointertoNodestart;
typedef PointertoNodestart NODESTART;

struct graph
{
    int V;
    struct nodestart *List;

};

typedef struct graph *PointertoGraph;
typedef PointertoGraph GRAPH;


struct graph *makeGraph(int n)
{
    int i;

    NODESTART L;
    L=(NODESTART)malloc(sizeof(struct nodestart)*(n+2));

    for(i=0;i<=n;i++)
        (L[i]).head=NULL;


    struct graph *G;
    G=malloc(sizeof(struct graph));

    G->List=L;
    G->V=n;

    L=G->List;

    return G;

};

void PrintDetailGraph(GRAPH G)
{
    int i,j,n;
     NODESTART P;
     NODE Q;

     n=G->V;

      printf("\n_____________________________________________________\n");

    for(i=1;i<=n;i++)
    {
        printf("%d--->",i);

        P=((G->List)+i);

        Q=P->head;

        while(Q!=NULL)
        {
            printf("%15d",Q->value);
            Q=Q->next;
        }

        printf("   X\n      ");

        Q=P->head;

        while(Q!=NULL)
        {
            printf("%15u",Q);
            Q=Q->next;
        }

        printf(" NULL\n");
    }

    printf("\n_____________________________________________________\n");
}


void PrintGraph(GRAPH G)
{
    int i,j,n;
     NODESTART P;
     NODE Q;

     n=G->V;

      printf("\n_____________________________________________________\n");

    for(i=1;i<=n;i++)
    {
        printf("%2d--->",i);

        P=((G->List)+i);

        Q=P->head;

        while(Q!=NULL)
        {
            printf("  %d",Q->value);
            Q=Q->next;
        }

        printf("   X\n");


    }

    printf("\n_____________________________________________________\n");
}


void Join(int x,int y,GRAPH G)
{
    NODE P;
    NODE Q;  int w;

    if((((G->List)[x]).head)==NULL)
      {
          P=malloc(sizeof(struct node));
          P->value=y;
          P->next=NULL;
          ((G->List)[x]).head=P;
      }

    else
    {
        Q=((G->List)[x]).head;

        w=1;
        while(Q->next!=NULL)
         {

             if(Q->value==y)
               {
                   w=0;  break;
               }

               Q=Q->next;

         }

             if(Q->value==y)
               {
                   w=0;
               }

         if(w==1)
         {

        P=malloc(sizeof(struct node));
          P->value=y;
          P->next=NULL;
          Q->next=P;
         }
    }

}

int main()
{

    int v; int i; int q;
    int X,Y;

    GRAPH G;

    scanf("%d",&v);

    G=makeGraph(v);

    NODESTART L;
    L=(G->List);


    PrintGraph(G);

    scanf("%d",&q);

    while(q--)
    {
        scanf("%d %d",&X,&Y);

        Join(X,Y,G);
        Join(Y,X,G);

        PrintGraph(G);


    }

    return 0;
}
