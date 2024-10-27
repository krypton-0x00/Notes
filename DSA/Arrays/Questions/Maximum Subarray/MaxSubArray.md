[1,2,3,4,5] -> continuous -> if we get the start and the end we can get the whole sub array

lets print all the sub arrays using the about information
```cpp
#include<iostream>
#include<vector>

using namespace std;

void maxsubarray(vector<int>arr){
     for(int start = 0 ; start< arr.size(); start++){
         for(int end = start; end<arr.size();end++){
             //if we have start and end we can get all the values bw them.
             for(int i = start ; i<=end; i++){ 
                 cout<<arr[i];
             } 
             cout<<endl;
         }
     }
    }


int main(){
    vector<int>nums = {1,2,3,4,5};
    maxsubarray(nums);    
   
}

```

# lets go back to the problem
Maximum sub array sum

### Bruteforce TC O(n^2)
[ 3, -4, 5, 4, -1, 7,  -8 ]
eg
[3, -4] = -1
[5,4,-1] = 8
we can have many sub arrays.

```cpp
 
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

void maxsubarray(vector<int>arr){
    int maxSum = INT_MIN;
     for(int start = 0 ; start< arr.size(); start++){
         int currentSum = 0; //should start from zero for ever sub array.
         for(int end = start; end<arr.size();end++){
            currentSum += arr[end];  //we have the start and end so we dont need the third loop to print coz we have the addition of the previous values we can directly add the next subarray element to it.
            maxSum = max(currentSum,maxSum);
         }
     }
     cout<<maxSum<<endl;

    }


int main(){
    vector<int>nums = {1,2,3,4,5};
    maxsubarray(nums);    
   
}
```

# Optimized Approach O(n)
Using Kadane's Algorithm
Intuition 
if we have a

| Number | operation | Number    | result |
| ------ | --------- | --------- | ------ |
| +ve    | +         | +ve       | +ve    |
| -ve    | +         | +ve (big) | +ve    |
| +ve    | +         | -ve (big) | -ve    |
if we have a big -ve number sum with small +ve number we get a -ve number .

it states
if your subarray sum becomes -ve reset **it to zero** coz its better to add a zero then a -ve number

maxSum = 
currentSum = 

loop {
	over the array;
	 currentSum += arr[i];
	 if(currentSum < 0) currentSum = 0;
}![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-1.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-2.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-3.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-4.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-5.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-6.webp]]
![[Maximum-Subarray-Sum-using-Kadanes-Algorithm-7.webp]]
```cpp

 
#include<iostream>
#include<vector>

using namespace std;

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = INT_MIN;
        int currentSum = 0;
        for(int i = 0 ; i< nums.size(); i++){
           
            currentSum += nums[i];
            maxSum = max(currentSum,maxSum);
            if(currentSum < 0) currentSum = 0;
        }
        return maxSum;
    }
};


int main(){
    vector<int>nums = {1,2,3,4,5};
    maxsubarray(nums);    
   
}

```