# polynomial-multiplication
#include<stdio.h>
#include<stdlib.h>

struct poly 
{
    int cf, px, py, pz;
    struct poly *link;
};

typedef struct poly* P;

P getnode() 
{
    P temp;
    temp = (P)malloc(sizeof(struct poly));
    return temp;
}

void read_poly(P p1, int n)
{
    P temp, next;
    int i;
    for (i = 0; i < n; i++)
    {
        temp = getnode();
        printf("Enter cf, px, py, pz: ");
        scanf("%d%d%d%d", &(temp->cf), &(temp->px), &(temp->py), &(temp->pz));
        next = p1->link;
        p1->link = temp;
        temp->link = next;
    }
}

void display(P p1) 
{
    P cur = p1->link;
    if (p1->link == p1) 
    {
        printf("Empty polynomial\n");
        return;
    }
    printf("The polynomial is: ");
    while (cur != p1) 
    {
        if (cur->cf > 0) 
        {   
            printf("+%dx^%dy^%dz^%d", cur->cf, cur->px, cur->py, cur->pz);
        } else 
        {
            printf("%dx^%dy^%dz^%d", cur->cf, cur->px, cur->py, cur->pz);
        }
        cur = cur->link;
    }
    printf("\n");
}

P compare(P term, P p2) 
{
    P cur = p2->link;
    while (cur != p2) 
    {
        if (cur->px == term->px && cur->py == term->py && cur->pz == term->pz) 
        {
            return cur;
        }
        cur = cur->link;
    }
    return NULL;
}

void insert(P p3, int cf, int px, int py, int pz) 
{
    P temp, next;
    temp = getnode();
    temp->cf = cf;
    temp->px = px;
    temp->py = py;
    temp->pz = pz;
    next = p3->link;
    p3->link = temp;
    temp->link = next;
}

void add_poly(P p1, P p2, P p3) 
{
    P next, cur;
    next = p1->link;
    while (next != p1) 
    {
        P cur = compare(next, p2);
        if (cur == NULL) 
        {
            insert(p3, next->cf, next->px, next->py, next->pz);
        }
        else 
        {
            insert(p3, next->cf + cur->cf, next->px, next->py, next->pz);
            cur->cf = -999;
        }
        next = next->link;
    }
    cur = p2->link;
    while (cur != p2) 
    {
        if (cur->cf != -999) 
        {
            insert(p3, cur->cf, cur->px, cur->py, cur->pz);
        }
        cur = cur->link;
    }
    display(p3);
}

int main() 
{
    P p1, p2, p3;
    p1 = getnode();
    p2 = getnode();
    p3 = getnode();
    p1->link = p1;
    p2->link = p2;
    p3->link = p3;
    int n1, n2;
    printf("Enter the number of terms in polynomial 1 and polynomial 2: ");
    scanf("%d%d", &n1, &n2);
    read_poly(p1, n1);
    display(p1);
    read_poly(p2, n2);
    display(p2);
    add_poly(p1, p2, p3);
    return 0;
}
