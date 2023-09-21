```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct NODE
{
    struct NODE* left;
    struct NODE* right;
    int key;
}NODE;


NODE* add(NODE* root , int szdata)
{
    NODE* p = root;
    NODE* parent = NULL;

    while(p != NULL)
    {
        parent = p;
        if(p->key > szdata)
            p = p->left;
        else if(p->key < szdata)
            p = p->right;
    }

    NODE* NEWNODE = (NODE*)malloc(sizeof(NODE));
    NEWNODE->key = szdata;
    NEWNODE->right = NEWNODE->left = NULL;

    if(parent == NULL)
    {
        return NEWNODE;
    }


    if(parent->key < szdata)
    {
        parent->right = NEWNODE;
        return NEWNODE;
    }
    else if(parent->key  > szdata)
    {
        parent->left = NEWNODE;
        return NEWNODE;
    }
}


void print(NODE* root)
{
    if(root == NULL)
    {
        return;
    }
    print(root->left);
    printf("%d\n" , root->key);
    print(root->right);
}

NODE* search(NODE* root , int szdata)
{
    NODE* p = root;
    NODE* parent = NULL;
    if(root == NULL)
    {
        printf("해당 트리는 비어있습니다.\n");
    }
    while(p != NULL)
    {
        parent = p;
        if(p->key == szdata)
            return p;
        else if(p->key > szdata)
            p = p->left;
        else if(p->key < szdata)
            p = p->right;
    }
    return NULL;

}

NODE* minNODE(NODE* root)
{
    NODE* p = root;
    NODE* parent = NULL;

    while(p != NULL)
    {
        parent = p;
        p = p->left;
    }
    return parent;
}


void Delete(NODE* root , int szdata)
{
    NODE* curr = root;
    NODE* parent = NULL;

    while(curr != NULL && curr->key != szdata)
    {
        parent = curr;
        if(curr->key > szdata)
            curr = curr->left;
        else if(curr->key < szdata)
            curr = curr->right;
    }
    
    if(curr == NULL)
    {
        printf("해당 노드가 존재하지 않습니다.\n");
        return;
    }
        



    if(curr->left == NULL && curr->right == NULL)
    {
        curr->left = curr->right = NULL;
        free(curr);
        if(parent->right == curr)
            parent->right = NULL;
        else if(parent->left == curr)
            parent->left = NULL;
        return;
    }
    else if(curr->left == NULL && curr->right != NULL)
        if(parent->right == curr)
        {
            parent->right = curr->right;
            free(curr);
            return;
        }
        else if(parent->left == curr)
        {    
            parent->left = curr->right;
            free(curr);
            return;
        }
    else if(curr->left != NULL && curr->right == NULL)
        if(parent->right == curr)
        {
            parent->right = curr->left;
            free(curr);
            return;
        }
        else if(parent->left == curr)
        {
            parent->left = curr->left;
            free(curr);
            return;
        }
    else if(curr->left != NULL && curr->right != NULL)
    {
        if(parent->right == curr)
        {
            parent->right = minNODE(curr->right);
            free(curr);
            return;
        }
        else if(parent->left == curr)
        {
            parent->left = minNODE(curr->right);
            free(curr);
            return;
        }
    }
}


int main()
{
    NODE* root = add(NULL , 1);
    add(root, 2);
    add(root, 3);
    add(root, 4);
    add(root, 5);
    add(root, 6);
    add(root, 7);
    add(root, 8);
    add(root, 9);
    

    Delete(root , 8);
    Delete(root , 3);
    Delete(root , 2);
    Delete(root , 10);
    print(root);


    printf("\n");
    return 0;
}
```
