Object[] a = new Object[1];
Integer b=1;
a[0]=b;
Integer[] c = (Integer[]) a;

possible object array contains not just integer



Object[] a = new Integer[1];
Integer b=1;
a[0]=b;
Integer[] c = (Integer[]) a;


https://stackoverflow.com/questions/1115230/casting-object-array-to-integer-array-error

Oject[] a = new Object[1];
Integer b=1;
a[0]=b;
T[] c = (T[]) a;



each object has it's type stored in the header of that object. When you cast a reference to an object, this doesn't alter the object in any way.

When you call .toString for example, it looks up the method to call from the classes' description. NOTE: It doesn't have to do this every time if it can optimise the code.

how is casting implemented in jvm??  checkcast
type erasure

https://stackoverflow.com/a/41531921/2797254
SomeOtherObject obj = arr.get(2); //trying to access the third element
You will definitely get the compile-time error. OK now try to explicitly cast the object returned.

SomeOtherObject obj = (SomeOtherObject)arr.get(2);
It works perfectly fine! Why? Because the object returned from get() method was compatible to the type it was being casted.

Now consider this also (I know I can't do it), just for checking if JVM can tell us which object is what.

SomeOtherObject obj = (SomeOtherObject)arr.get(1); //1 instead of 2
Yes yes I know second member of this array is a String and I cannot do this. But lets see what has JVM has to say (there is no compiler error though). The message we get is that the object cannot be cast to the type specified. (But we do not get the exact type of the object).

Did you get the point? JVM here only knows that the objects being passed to this array are just objects compatible with Object. It has no idea if we actually passed the String, an int (or rather Integer, because primitives cannot be stored in collections), or SomeOtherObject (any class defined in Java is immediate subclass of Object)
