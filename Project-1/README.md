1. Which Program is fastest and is it always fastest?

   in small payloads (3-100 bytes per node) alloc was fastest at 1.37s using 903,168 kb of memory -------------- NUM_BLOCKS = 10,000,000
   in large payloads (4096 bytes per node) malloc was fastest at 2.09s usiong 1,210,752 kb of memory -------------- NUM_BLOCKS = 200,000

   alloc is fastest when node payload is small because it avoids heap allocation overhead, but its not always the fastest



2. Which program is slowest and is it always slowest?

   for small payloads, list.cpp (while using std::list) was the slwoest, it was also slowest for larger payloads, but the performance differences narrowed and no implementation was much slower

   small payload ---- 2.03s ________ large payload ----- 2.12



3. Was there a trend in program execution time based on the size of data in each Node? If so, what, and why?

    As node data size increased, runtimes increased substantially, differnce between implimentations shrank, allocation overhead became much less important, and hash computatrion dominates execution time

   When nodes store only a few bytes alocation cost and pointers overhead are large fraction of total runtime

   when nodes store 4096 bytes most time is spent hashing over the large memory blocks and allocation becomes a small percentage of the total work done



4. Was there a trend in program execution time based on the length of the block chain?

  test were run fro 10,000 to 10,000,000 nodes

  Runtime increased pretty linearly with NUM_BLOCKS

  memory usage increases linearly

  relative ranking of implementations remained similar at small payload sizes

  with the linear trend of memory and runtime, it matches the expected behavior of O(n)---> one allocation per node, and one hash pass over all nodes



5. Consider heap breaks, what's noticeable? Does increasing the stack size affect the heap? Speculate on any similarities and differences in programs?

  | NUM_BLOCKS | alloca | list | malloc | new |
  | ---------- | ------ | ---- | ------ | --- |
  | 10,000     | 1      | 1    | 1      | 1   |
  | 100,000    | 1      | 1    | 1      | 1   |
  | 1,000,000  | 1      | 1    | 1      | 1   |
  | 5,000,000  | 1      | 1    | 1      | 1   |
  | 10,000,000 | 1      | 1    | 1      | 1   |

  heap growth events were consistant overall, even millions of allocations didnt change the break count

  Inceasing the stack size does not affect the heap

  stack and heap are seperate memory reagioons, increasing the stack limits (ulimit -s unlimited) only changes the stack growth, not the heap allocation behavior

  

6. Considering either the malloc.cpp or alloca.cpp versions of the program, generate a diagram showing two Nodes. Include in the diagram the relationship of the head, tail, and Node next pointers. show the size (in bytes) and structure of a Node that allocated six bytes of data

  for the malloc.cpp version, a diagram would look something like ---->             head --> [Node A] --> [Node B] --> null

  the internal stucture of one node assuming 8-byte pointer and 6 bytes of allocated data would loock something like  | next pointer 8 bytes   |
                                                                                                                      -------------------------
                                                                                                                      | size (size_t, 8 bytes) |
                                                                                                                      -------------------------
                                                                                                                      | bytes* pointer (8 B)   |
                                                                                                                      --------------------------
the allocated data (6 bytes) would look like this:

  Heapo Allocation (6 bytes) ------------------------------------
                            | b0  | b1  | b2  | b3  | b4  | b5  |
                            -------------------------------------
                              ^---- Byte pointer points to b0

and the Head and tail relationship would just be like this: 
                                                                  head --> Node A --> Node B --> NULL
                                                                  tail ------------------------^
                            head points to first node, each node's next pointer links to the next, the final node's next is null, and the tail references the last node

                            

7. There's an overhead to allocating memory, initializing it, and eventually processing (in our case, hashing it). For each program, were any of these tasks the same? Which one(s) were different?

  The things that were the same across all porgrams were the data initialization, hash compujtation, and one traversal of the linked list

  The things that were different between programs were the Allocation mechanics, memory layouts, cache locality, recusion(alloca version) and container overhead (std::list)
  

8. As the size of data in a Node increases, does the significance of allocating the node increase or decrease?

   Allocation significantly decreases as node size increases, this is because the allocation cost is relatively constant per node, but the hashing cost increased with data size.

    As the date grows, hashing dominates total runtime, the allocation method becomes way less importatnt

















