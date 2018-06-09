# Vector  
Vector are **sequence containers** representing arrays that **can change in size**.  
> 在C++中，Vector是一个十分有用的**序列容器**，简单来说，Vector是一个能够**存放任意类型的动态数组**，数组的大小是可以改变的。
  
Just like arrays, vectors use **contiguous storage locations** for their elements, which means that their elements can also be accessed using **offsets on regular pointers to its elements**, and just as efficiently as in arrays. But unlike arrays, their **size can change dynamically**, with their storage being handled automatically by the container.  
> 元素的储存：contiguous storage location  
> 元素的访问：除了像数组一样通过下标以外，offsets on regular pointers to its elements也可以  
> 数组的大小：是可以动态改变的，container会处理好。

  
  ### Container properties
  1. Sequence  
  Elements in sequence containers are ordered in a strict linear sequence. Individual elements are accessed by their position in this sequence.  
  > 元素在sequence container里是线性排序的。
  > 像数组一样，元素可通过其在序列里的顺序被访问到。
  2. Dynamic array  
  Allows direct access to any element in the sequence, even through pointer arithmetics, and provides relatively fast addition/removal of elements at the end of the sequence.  
  > 在end of the sequence，提供了增删操作。
  > 且尽管是动态的数组，元素的访问也可以通过pointer arithmetics。
  3. Allocator-aware  
  The container uses an allocator object to dynamically handle its storage needs.  
  > 容器使用分配器对象（allocator object）来动态处理其储存需求。
