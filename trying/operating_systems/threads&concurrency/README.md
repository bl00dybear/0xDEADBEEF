# Practice Exercises for Threads
    
1. Provide three programming examples in which multithreading provides better performance than a single-threaded solution.


    1.1. Web Server Handling Multiple Client Requests

    Description: In a web server, multiple clients can send requests simultaneously (for example, loading different pages, resources, etc.). A single-threaded server would process each request    sequentially, which means other clients have to wait for the previous ones to finish.

    Multithreading Advantage: With multithreading, the server can handle multiple requests at the same time by assigning each request to a separate thread. This allows multiple clients to be served concurrently, leading to faster response times and more efficient resource utilization.

    1.2. Image Processing (Applying Filters or Transformations)

    Description: Image processing tasks, such as applying filters (e.g., blur, sharpen) or transformations (e.g., rotation, scaling), can be very computationally intensive, especially for large images. A single-threaded solution would apply the filter to each pixel one at a time, which can be very slow.

    Multithreading Advantage: With multithreading, the image can be divided into multiple parts (e.g., by rows or columns), and each part can be processed concurrently in separate threads. This reduces the total processing time by utilizing multiple CPU cores.

    1.3. Sorting Large Datasets (Parallel Merge Sort or Quick Sort)

    Description: Sorting is a common task in computing, and sorting large datasets can take significant time in a single-threaded approach, as the algorithm processes one portion of data at a time.

    Multithreading Advantage: In parallel sorting (e.g., parallel merge sort or quicksort), the dataset can be divided into smaller parts, each sorted independently in separate threads. After sorting the parts, they are merged to produce the final sorted dataset. This approach can greatly reduce sorting time, especially on multi-core systems.


2. Using Amdahl’s Law, calculate the speedup gain of an application that has a 60 percent parallel component for (a) two processing cores and (b) four processing cores.

3. Does the multithreaded web server described in Section 4.1 exhibit task or data parallelism?

4. What are two differences between user-level threads and kernel-level threads? Under what circumstances is one type better than the other?

5. Describe the actions taken by a kernel to context-switch between kernel- level threads.

6. What resources are used when a thread is created? How do they differ from those used when a process is created?

7. Assume that an operating system maps user-level threads to the kernel using the many-to-many model and that the mapping is done through LWPs. Furthermore, the system allows developers to create real-time threads for use in real-time systems. Is it necessary to bind a real-time thread to an LWP? Explain.
