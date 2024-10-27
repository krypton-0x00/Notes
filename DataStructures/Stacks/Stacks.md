```cpp
#include <iostream>
#include<stack>

void printElements(std::stack<int>stack) {
    while (!stack.empty()) {
        std::cout << stack.top() << std::endl;
        stack.pop();
    }
}
int main()
{
    //Empty , Size, push , pop , top
    std::stack<int>numberStack;
    numberStack.push(12);
    numberStack.push(13);
    numberStack.push(14);
    if (numberStack.empty()) {
        std::cout << "Stack is empty" << std::endl;
    }
    else {
        std::cout <<"ITEMS: "<<numberStack.size() << std::endl; //3
        std::cout << "PEAK:" << numberStack.top() << std::endl; //14 coz 14 is at the top
    }
    printElements(numberStack);
    numberStack.pop();//removes last element ie 14;


}
```