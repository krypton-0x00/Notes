`sprinf` is going to simple write but instead of writing to the console it prints to a string buffer 

we got a buffer string here and `sprintf` is going to put the result to formatting these two strings next to each other.
```c


char a[] = {'h','i'};
char b[] = {'f','i'};
char buffer[5] ={0};
sprintf(buffer, "%s%s",a,b);
```