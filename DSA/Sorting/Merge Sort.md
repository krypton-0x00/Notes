Merge to sorted arrays code
- e.g:
- arr 1 = {1,3,5,7};
- arr 2 = {2,4,6,8};

result = {1,2,3,4,5,6,7,8};


```cpp
vector<int> merge(vector<int> arr1,vector<int> arr2){
    vector<int>result(6,0);
    int i = 0; //arr1 index
    int j = 0; //arr2 index
    int k = 0; //result index
    
    while(i < arr1.size() && j < arr2.size()){
        if(arr1[i]>arr2[j]){
            result[k] = arr2[j];
            j++;
        }else{
            result[k] = arr1[i];
            i++;
        }
        k++;
    }
    //if the above loop stops and there are still some elements in arr1 this will add them to the end;
    while(i < arr1.size()){
        result[k] = arr1[i];
        k++;
        i++;
    }
    while(j < arr2.size()){
        result[k] = arr2[j];
        k++;
        j++;
    }
    
    
    for(int num : result){
        cout<<num<<", "<<std::endl;
    }
    
    return result;
}


```
# Merge Sort
```cpp
void merge(vector<int>&arr,int low, int mid , int high){
        vector<int>temp;
        int left = low;
        int right = mid + 1;
        while(left <= mid && right <= high){
            if(arr[left]<=arr[right]){
                temp.push_back(arr[left]);
                left++;
            }else{
                temp.push_back(arr[right]);
                right++;
            }

        }
        while(left<=mid){
            temp.push_back(arr[left]);
            left++;
        }
        while(right<=high){
            temp.push_back(arr[right]);
            right++;
        }
        for(int i = low ; i<=high;i++){
            arr[i] = temp[i - low];
        }
        
    }
    void mergeSort(vector<int> &arr, int low, int high){
        if(low >= high) return;

         int mid = (low+high) / 2;
         mergeSort(arr,low,mid);
         mergeSort(arr,mid+1,high);
         merge(arr,low ,mid, high);

    }
     
```