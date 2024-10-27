Basic implementation
```cpp
#inlcude<iostream>
struct Node {
	int Value;
	Node* Next;
}

void printList(Node* firstNode){
	while(firstNode != NULL){
		std::cout<<firstNode.Value<<std::endl;
		firstNode =  firstNode->Next;
	}
	
}

int main(){
	Node* head = new Node();
	Node* second = new Node();
	Node* third = new Node();
//set values
	head->Value = 1;
	second->Value = 2;
	third->Value = 3;
	//ptrs
	head->Next = second;
	second->Next = third;
	third->Next = NULL;

}

printList(head);


```
1 ->  2 -> 3

# Insert and Push to the LinkedList;
```cpp
#include<iostream>

class Node {
public:
	int value;
	Node* next;
};





void printNode(Node* node) {
	while (node != NULL) {
		std::cout << node->value << "->" << (node->next == NULL ? "NULL":"");
		node = node->next;
	}
}





//insert at front
void insert(Node** head,int val) {
	Node* newNode = new Node();
	newNode->value = val;
	newNode->next = *head;
	*head = newNode;

}




void push(Node**head, int val) {
	//Prepare a node
	Node* newNode = new Node();
	newNode->value = val;
	newNode->next = NULL;
	//if LL is empty 
	if (*head == NULL) {
		*head = newNode;
		return;
	}
	//find the last node
	Node* last = *head;
	while (last->next != NULL)
	{
		last = last->next;
	}

	//Insert newNode at he end;
	last->next = newNode;
	 
}





void insertAt(Node* prev, int val) {
	//1. Check if the previous node is NULL
	if (prev == NULL)return;

	//2. Prepare a New Node
	Node* newNode = new Node();
	newNode->value = val;
	newNode->next = prev->next;

	//3.Insert new node after prev
	prev->next = newNode;
}






void pop(Node**head) {
	if (*head == NULL)return;
	if ((*head)->next == NULL) {
		delete* head; //Delete the single node
		*head = NULL;  // Update head to NULL
		return;

	}

	Node* last = *head;
	Node* secondLast = 0;
	while (last->next != NULL)
	{
		secondLast = last;
		last = last->next;
	}
	
	secondLast->next = NULL;
	
	delete last;

}





void erease(Node* prev) {
	Node* current = prev->next;
	prev->next = current->next;
	delete current;
}





int main() {
	Node* head = new Node();
	Node* second = new Node();
	Node* third = new Node();
	//set values
	head->value = 1;
	second->value = 2;
	third->value = 3;
	//ptrs
	head->next = second;
	second->next = third;
	third->next = NULL;

	//insert(&head, 8);
	//push(&head, 12);
	//insertAt(second, 300);
	pop(head);
	//erease(head);

	printNode(head);

}
```