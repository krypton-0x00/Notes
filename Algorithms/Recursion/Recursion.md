![[Pasted image 20241013120425.png]]
# Reverse a string using Recursion

```cpp
#include <iostream>

void revString(std::string str){
    if(str.size() == 0) return ;
    
    revString(str.substr(1)); //substring(1)   "hakir"
    std::cout<<str[0];
}
 
int main(){
    std::string name = "shakir";
    revString(name);
}

```
# Reverse a Vector without recursion
```cpp
#include <iostream>
#include <vector>
using namespace std;
void reverseString(vector<char>& s) {
    int end = s.size();
    for (int i = 0; i < s.size() / 2; i++) {
        swap(s[i], s[end]);
        end--;
    }
}

int main() {
    vector<char> str = { 's','h','a','k','i','r' };
    reverseString(str);
    for (auto i : str) {
        cout << i;
    }
}
```