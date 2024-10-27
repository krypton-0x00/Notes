```cpp
 
#include <iostream>
#include<vector>

int main() {
    std::vector<int>arr = {1,4,2,6,7,3};
    bool swapped = false;
    for(int i = 0; i<arr.size();i++){
        for(int j = 0; j<arr.size()-1-i;j++){ 
            if(arr[j]>arr[j+1]){
                std::swap(arr[j],arr[j+1]);
                swapped = true;
            } 
        }
        if (!swapped) break;
    }
    
    
    for(auto i : arr){
        std::cout<<i<<" ";
    }

    return 0;
}
```