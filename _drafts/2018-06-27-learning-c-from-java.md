<pre>
c:

int a = 10;

a variable represents the box itself, it doesn't exist after compilation
-------
|  a  |
-------

every box has an address. &a

Whenever you create a box, there is an address associated with it.

int a = 10;
a = 20;
this change the box value, doesn't change the box address.

box = value  //this copies value into box
pointer = address;  //this update pointer with new pointee


Use pointer to manipulate the address. Use variable to manipulate the box content.


address is the same value as a pointer.

int a = 10;
int* p = &a;

a stores the box
p stores the address
p == &a ;
*p == a ;

Arrays

int array[6];

array[3] refers to the box.
array + 3 * 8 refers to the address



</pre>


why do we need malloc
I got the stack vs. heap allocation part. but people are explaining the dynamic part like:

malloc is used for dynamic memory allocation, which means you allocate the memory at run time. For example when you don't know the amount of memory during compile time.


```
#include <iostream>
#include <ctime>

using namespace std;

double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};

double& setValues( int i ) {
   return vals[i];   // return a reference to the ith element
}

// main function to call above defined function.
int main () {

   setValues(1) = 20.23; // change 2nd element

   double& val = setValues(1);
   val = 9999;
   cout << vals[1] << endl;

   return 0;
}
```



Async server jetty: > 9.0, or netty
-> have a job queue and event loop

need async flow: parseq
-> be able to put job into job queue.


parrelle vs concurrency


implement a read write lock
