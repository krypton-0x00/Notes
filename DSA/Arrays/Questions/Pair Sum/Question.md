Given a sorted array and a target x number we need to return a pair of 2 numbers whose sum is equal to x;
[2, 3, 11, 15]  target 5

answer: [2 , 3]
Brute Force O(n^2)
```cpp
#include<bits/stdc++.h>

using namespace std;
void pairSum(vector<int>nums,int target){
    for(int start = 0; start < nums.size();start++){
        for(int end = start; end < nums.size();end++ ){
            if(nums[start]+nums[end] == target){
                 cout<<"("<<start<<","<<end<<")"<<endl;   
            }
        }
    }

}

int main(){
    vector<int>nums = {2,4,5,7,9,10};
    pairSum(nums,9);
}
```

Optimal Way
We know this array is sorted
we can use two pointer approach 
1. Sum > target -> j-- 
2. Sum < target -> i++
3. Sum = target -> answer
Because its a sorted array so i.e at the end on j pointer we will have larger values so ie. if the sum is greater then the target we need a smaller value at the j pointer .
```cpp
#include<bits/stdc++.h>

using namespace std;
void pairSum(vector<int>nums,int target){
    int start = 0;
    int end = nums.size() - 1;
    while(start<end){      
         if(nums[start] + nums[end] > target){
            end--;
         }
         else if(nums[start] + nums [end]< target){
            start++;
         }
         else{
            cout<<start<<","<<end<<endl; 
            break;
         }
          
    }

}

int main(){
    vector<int>nums = {0,1,2,4,5,7,9,10};
    pairSum(nums,9);
}
```