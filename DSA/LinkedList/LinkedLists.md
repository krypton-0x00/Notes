![[Pasted image 20240921124032.png]]
```cpp
#include<iostream>
#include<vector>
using namespace std;

class Node{
public:
    int data;
    Node* next;

    Node(int data1,Node* next1){
        data = data1;
        next = next1;
    }

};
int main(){
    vector<int>arr= {1,2,3,4,5};
    Node* y = new Node(arr[0],nullptr); //0x64e639ce22d0
    cout<<(y->data);
}
```