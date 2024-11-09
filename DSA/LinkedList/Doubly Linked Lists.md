    1->2->3->4->null
 null<- <- <-<-



```cpp
struct Node{
	int value;
	Node* next;
	Node* previous;
	
	Node(int value){
		data = value;
		next = nullptr;
		previous = nullptr;
	}
}
void insertFront(Node*& head, int value) {
    Node* newNode = new Node(value);
    if (head == nullptr) {
        head = newNode;
    } else {
        newNode->next = head;
        head->prev = newNode;
        head = newNode;
    }
}

void insertEnd(Node*& head, int value) {
    Node* newNode = new Node(value);
    if (head == nullptr) {
        head = newNode;
    } else {
        Node* temp = head;
        while (temp->next != nullptr) {
            temp = temp->next;
        }
        temp->next = newNode;
        newNode->prev = temp;
    }
}
void deleteNode(Node*& head, int value) {
    Node* temp = head;
    
    // Case 1: Empty list
    if (head == nullptr) return;

    // Case 2: Deleting the head node
    if (head->data == value) {
        head = head->next;
        if (head != nullptr) {
            head->prev = nullptr;
        }
        delete temp;
        return;
    }

    // Case 3: Deleting a node in between or at the end
    while (temp != nullptr && temp->data != value) {
        temp = temp->next;
    }

    if (temp == nullptr) return; // Node not found

    if (temp->next != nullptr) {
        temp->next->prev = temp->prev;
    }

    if (temp->prev != nullptr) {
        temp->prev->next = temp->next;
    }

    delete temp;
}


void traverseForward(Node* head) {
    Node* temp = head;
    while (temp != nullptr) {
        std::cout << temp->data << " ";
        temp = temp->next;
    }
    std::cout << std::endl;
}
void traverseBackward(Node* head) {
    if (head == nullptr) return;
    Node* temp = head;
    
    // Move to the last node
    while (temp->next != nullptr) {
        temp = temp->next;
    }
    
    // Traverse backwards
    while (temp != nullptr) {
        std::cout << temp->data << " ";
        temp = temp->prev;
    }
    std::cout << std::endl;
}



int main() {
    Node* head = nullptr;

    insertFront(head, 10);
    insertFront(head, 20);
    insertEnd(head, 30);
    insertEnd(head, 40);

    std::cout << "Forward Traversal: ";
    traverseForward(head);

    std::cout << "Backward Traversal: ";
    traverseBackward(head);

    deleteNode(head, 20);
    std::cout << "After deletion of 20: ";
    traverseForward(head);

    return 0;
}


Forward Traversal: 20 10 30 40 
Backward Traversal: 40 30 10 20 
After deletion of 20: 10 30 40 




```