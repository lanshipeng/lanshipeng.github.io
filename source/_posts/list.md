---
title: 单链表
categories: 
- 数据结构
tags:   链表 
comments: true
---
#### 单链表的创建、插入、删除 倒置

- go
  
```
    type ListNode struct{
        value int
        next *ListNode
    }
    //创建单链表
    func (this *ListNode)CreateList()*ListNode{
        
        head := new(ListNode)
        ptail := new(ListNode)
        ptail = head
        for i :=0; i <= 10; i++{
             xNode := new(ListNode)     
             xNode.value = i
             xNode.next = nil 
             ptail.next = xNode
             ptail = xNode
        }
        return phead
    }
    //删除节点
    func (this *ListNode)DeleteListNod(head* ListNode,val int){  
        if head == nil{
            return nil
        }

        temp := new(ListNode)
        p := new(ListNode)
        p = head
        for p.next != nil && p.value != val{
            temp = p
            p = p.next
        }
        if p.value == val{
            if p == head{
                head = temp.next
            }else{
                temp.next = p.next
            }
        }
        return head
    }
    //插入节点
    func (this *ListNode)InsertNode(head* ListNode,val int)*ListNode{
        if head == nil{
            return
        }
        xNode := new(ListNode)
        xNode.value = val
        xNode.next = nil

        temp := new(ListNode)
        p := new(ListNode)
        p = head
        for p.next != nil && p.value < val{
            temp = p
            p = p.next
        }
        if p.value <= val{
            if p == head{
                head = xNode
                xNode.next = p
            }else{
                temp.next = xNode
                xNode.next = p
            }

        }else{
            p.next = xNode
            xNode.next = nil
        }
        return head
    }
    //链表倒置
    func (this *ListNode)Reverse(head* ListNode)*ListNode{
        if head==nil{
            return nil
        }
        temp := new(ListNode)
        p := new(ListNode)
        p = head.next
        head.next=nil
        for p != nil{
            temp = temp.next
            temp.next = head
            head = p
            p = temp
        }
        return head
    }
```
- c++

```
    struct ListNode{

        ListNode *next;
        int val;

    }
    class Solution{
    public:
        ListNode* CreateListNode(){

            ListNode* head =new ListNode;
            p = head;
            for (int i =0; i <10; i++){
                ListNode* node =new ListNode;
                node -> next =NULL;
                node -> val = i;
                p->next = node;
                p = node;
            }
            p->next = NULL;
            return head;

        }
        ListNode* DeletedListNode(ListNode* head,int val){
            if head == NULL{
                return NULL;
            }
            ListNode* temp = NULL;
            ListNode* p = head;
            while(p->next != NULL && p->val != val){
                temp = p;
                p = p->next;
            }
            if (p->val == val){
                if (p == head){
                    head = p->next;
                    delete p;
                }
                else{
                    temp->next = p->next;
                    delete p;
                }
            }
            return head;
        }
        ListNode* InsertListNode(ListNode* head,int val){

            ListNode* p = head;
            ListNode* temp = NULL;

            ListNode* node = new ListNode;
            node->val = val;

            while(p->next != NULL && p->val < node->val){
                temp = p;
                p= p->next;
            }
            if (node->val <= p->val){
                if (p == head){//头部插入
                    head = node;
                    node -> next = p;
                }else{//中间插入
                    temp -> next = node;
                    node -> next = p;
                }
            }else{//链表尾部插入
                p->next = node;
                node->next = NULL;
            }
            return head;
        }
        ListNode* ReverseListNode(ListNode* head){
            if (head == NULL){
                return NULL;
            }
            ListNode* temp = NULL;
            ListNode* p = head;
            head->next = NULL;
            while(p!=NULL){
                temp = p->next;
                p->next = head;
                head = p;
                p = temp;
            }
            return head;
        }
        ListNode* DeleteListNode(ListNode* head,int val){

            if (head == NULL) return NULL;
            ListNode* temp = NULL;
            ListNode* p = head;
            while(val < p->val && p->next !=NULL){
            temp = p;
            p = p->next; 
            }
            if (p->val == val){
                if (p == head){
                head = p->next;
                delete p;
                }else{
                    temp->next = p->next;
                    delete p;
                }
            }
        return head;
        }
        ListNode* InsertListNode(ListNode* head,int val){
            
            ListNode* temp = NULL;
            ListNode* p = head;

            ListNode* newNode = new ListNode;
            newNode->val = val;

            while(val < p->val && p->next !=NULL){
            temp = p;
            p = p->next; 
            }
            if (p->val <= newNode->val ){
                if(p == head){
                    head = newNode;
                    newNode->next = p; 
                }else{
                    temp->next = newNode;
                    newNode->next = p;
                }

            }else{//尾部插入
                p->next = newNode;
                newNode->next = NULL;
            }
        }
    }
```