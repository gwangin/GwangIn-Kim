"""C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct NODE
{
    struct NODE* next;
    struct NODE* represent;
    int data;
}NODE;




NODE* make_set(int n_data)
{
    NODE* head_node = (NODE*)malloc(sizeof(NODE));
    head_node->data = n_data;
    head_node->next = NULL;
    head_node->represent = head_node;
    return head_node;
}

void add_node(NODE* head_node , int n_data)
{
    if(head_node == NULL)
    {
        printf("해당 set가 존재하지 않습니다. set를 먼저 만들어주세요.\n");
        return;
    }

    NODE* n_NODE = (NODE*)malloc(sizeof(NODE));
    n_NODE->data = n_data;
    n_NODE->represent = head_node;
    n_NODE->next = NULL;

    NODE* p_NODE = head_node;
    while(p_NODE->next)
    {
        p_NODE = p_NODE->next;
    } 
    p_NODE->next = n_NODE;
    return;
}

void print_nodes(NODE* head_node)
{
    NODE* p_node =  head_node;
    while(p_node)
    {
        printf("대표노드 : %d  현재노드 : %d \n" , p_node->represent->data , p_node->data);
        p_node = p_node->next;
    }
}


int len_set(NODE* head_node)
{
    int len = 1;
    NODE* p_node = head_node;
    while(p_node->next)
    {
        len += 1;
        p_node = p_node->next;
    }
    return len;
}



NODE* link_sets(NODE* head_1 , NODE* head_2)
{   
    if(len_set(head_1) >= len_set(head_2))
    {   
        NODE* p_node = head_2;
        while(p_node->next)
        {
        p_node->represent = head_1;
        p_node = p_node->next;
        }
        p_node->represent = head_1;
        NODE* p_node_1 = head_1;
        while(p_node_1->next)
        {
            p_node_1 = p_node_1->next;
        }
        p_node_1->next = head_2;
        return head_1;
    }
    else if(len_set(head_1) < len_set(head_2))
    {
        NODE* p_node = head_1;
        while(p_node->next)
        {
            p_node->represent = head_2;
            p_node = p_node->next;
        }
        p_node->represent = head_2;
        NODE* p_node_1 = head_2;
        while(p_node_1->next)
        {
            p_node_1 = p_node_1->next;
        }
        p_node_1->next = head_1;
        return head_2;
    }


}



int main()
{
    NODE* set_1 = make_set(1);
    NODE* set_2 = make_set(11);
    add_node(set_1 , 2);
    add_node(set_1 , 3);
    add_node(set_1 , 4);
    add_node(set_1 , 5);
    add_node(set_2 , 12);
    add_node(set_2 , 13);
    add_node(set_2 , 14);
    add_node(set_2 , 15);
    add_node(set_2 , 16);
    print_nodes(set_1);
    print_nodes(set_2);

    putchar('\n');
    NODE* linked_set = link_sets(set_1 , set_2);
    print_nodes(linked_set);


    return 0;
}
"""
