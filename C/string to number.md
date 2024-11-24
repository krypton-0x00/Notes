```c
int str_to_int(char *str){
	int result = 0;
	for(int i = 0 ; str[i] != '\0'; i++){
		//check if if the char is not number
		if(str[i] < '0' && str[i] > '9')
			return -1;
			
		result = (result * 10) + (str[i] - '0');
	}
	return result;
}
```

# How it works
![[Pasted image 20241122114554.png]]
![[Pasted image 20241122114607.png]]
48 is the first numeric char that encodes zero in ascii
![[Pasted image 20241122114711.png]]
What about multi digit numbers

Lets take an example what is 4327

4327 = 4(10^3) + 3(10^2) + 3(10^1) + 7(10^0);

1. Convert each char to the number.
 ![[Pasted image 20241122114931.png]]
2.Do the same multiplication here.
![[Pasted image 20241122115009.png]]
3. Add all of them
![[Pasted image 20241122115112.png]]
4. We have a number.

