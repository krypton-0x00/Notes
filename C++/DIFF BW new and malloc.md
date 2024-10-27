new keyword uses malloc under the hood



```cpp
//new
Entity* e = new Entity(); //but here we can call the constructor
//under the hood 
Entity* e = (Entity*)malloc(sizeof(Entity)); //but here we can call the constructor we are just allocating a memory; thats the difference btw two;




```