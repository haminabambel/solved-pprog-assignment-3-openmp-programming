Download Link: https://assignmentchef.com/product/solved-pprog-assignment-3-openmp-programming
<br>
<span class="kksr-muted">Rate this product</span>

<main>







</main>

The purpose of this assignment is to familiarize yourself with OpenMP programming.

<hr>

Get the source code:

<pre><code class="shell language-shell">$ wget https://nycu-sslab.github.io/PP-f21/HW3/HW3.zip$ unzip HW3.zip -d HW3$ cd HW3</code></pre>

<h2 id="1part1parallelizingconjugategradientmethodwithopenmp">1. Part 1: Parallelizing Conjugate Gradient Method with OpenMP</h2>

Conjugate gradient method is an algorithm for the numerical solution of particular systems of linear equations. It is often used to solve partial differential equations, or applied on some optimization problems. You may get more information on <a href="https://en.wikipedia.org/wiki/Conjugate_gradient_method" target="_blank" rel="noopener">Wikipedia</a>.

Please enter the <code>part1</code> folder:

<pre><code class="shell language-shell">$ cd part1</code></pre>

You can build and run the CG program by:

<pre><code class="shell language-shell">$ make; ./cg</code></pre>

In this assignment, you are asked to parallelize a serial implementation of the conjugate gradient method using OpenMP. The serial implementation is in <code>cg.c</code> and <code>cg_impl.c</code>. Please refer to the <code>README</code> file for more information about the code.

In order to parallelize the conjugate gradient method, you may want to use profiling tools to help you explain the performance difference before and after your modification. As it probably just works ineffectively if you simply add a parallel-for directive for each <code>for</code> loop, you may want to use profiling tools, such as <code>perf</code>, <code>time</code>, <code>gprof</code>, <code>Valgrind</code> (not limited to these tools), to profile your program to better understand how much the improvement is from the code snippets you have modified.

The following instructions may help you finish this part:

<ul>

 <li>Try to find the hot spots of the program.</li>

 <li>A possible improvement might be from more data parallelism, code refactoring, less page fault, or something else.</li>

</ul>

See the <a href="#requirements">requirements</a> to finish this part.

You can also run our grader via: <code>./cg_grader</code>, which reports the correctness of your program and a performance score.

<h2 id="part2parallelizingpagerankalgorithmwithopenmp">Part 2: Parallelizing PageRank Algorithm with OpenMP</h2>

<img decoding="async" alt="image" data-recalc-dims="1" data-src="https://i0.wp.com/user-images.githubusercontent.com/18013815/95656789-47c8fa00-0b43-11eb-80f5-56b7a849a831.png?w=980&amp;ssl=1" class="lazyload" src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==">

 <noscript>

  <img decoding="async" src="https://i0.wp.com/user-images.githubusercontent.com/18013815/95656789-47c8fa00-0b43-11eb-80f5-56b7a849a831.png?w=980&amp;ssl=1" alt="image" data-recalc-dims="1">

 </noscript>

In this part, you will implement two graph processing algorithms: <a href="https://en.wikipedia.org/wiki/Breadth-first_search" target="_blank" rel="noopener">breadth-first search</a> (BFS) and a simple implementation of <a href="https://en.wikipedia.org/wiki/PageRank" target="_blank" rel="noopener">page rank</a>. A good implementation of this assignment will be able to run these algorithms on graphs containing hundreds of millions of edges on a multi-core machine in only seconds.

Please enter the <code>part2</code> folder:

<pre><code class="shell language-shell">$ cd part2</code></pre>

Read the following sections and accomplish the <a href="#requirements">requirements</a> of this part.

<h3 id="21backgroundrepresentinggraphs">2.1 Background: Representing Graphs</h3>

The starter code operates on directed graphs, whose implementation you can find in <code>common/graph.h</code> and <code>common/graph_internal.h</code>. We recommend you begin by understanding the graph representation in these files. A graph is represented by an array of edges (both <code>outgoing_edges</code> and <code>incoming_edges</code>), where each edge is represented by an integer describing the id of the destination vertex. Edges are stored in the graph sorted by their source vertex, so the source vertex is implicit in the representation. This makes for a compact representation of the graph, and also allows it to be stored contiguously in memory. For example, to iterate over the outgoing edges for all nodes in the graph, you’d use the following code which makes use of convenient helper functions defined in <code>common/graph.h</code> (and implemented in <code>common/graph_internal.h</code>):

<pre><code class="c++ language-c++">for (int i=0; i&lt;num_nodes(g); i++) {    // Vertex is typedef'ed to an int. Vertex* points into g.outgoing_edges[]    const Vertex* start = outgoing_begin(g, i);    const Vertex* end = outgoing_end(g, i);    for (const Vertex* v=start; v!=end; v++)        printf("Edge %u %u
", i, *v);}</code></pre>

<h3 id="22task1implementingpagerank">2.2 Task 1: Implementing Page Rank</h3>

As a simple warm up exercise to get comfortable using the graph data structures, and to get acquainted with a few OpenMP basics, we’d like you to begin by implementing a basic version of the well-known <a href="https://en.wikipedia.org/wiki/PageRank" target="_blank" rel="noopener">page rank</a> algorithm.

Please take a look at the pseudocode provided to you in the function <code>pageRank()</code>, in the file <code>page_rank/page_rank.cpp</code>. You should implement the function, parallelizing the code with OpenMP. Just like any other algorithm, first identify independent work and any necessary synchronization.

You can run your code, checking correctness and performance against the staff reference solution using:

<pre><code>./pr &lt;PATH_TO_GRAPH_DIRECTORY&gt;/com-orkut_117m.graph </code></pre>

If you are working on our workstation machines, we’ve located a copy of the graph and some other graphs at <code>/HW3/graphs/</code>. You can also download the graphs from <code>http://sslab.cs.nctu.edu.tw/~acliu/all_graphs.tgz</code>. (But be careful, this is a 3GB download. <strong>Do not replicate the data on the workstations or download the zipped tar file to the workstations for conservation of storage space.</strong>)

Some interesting real-world graphs include:

<ul>

 <li>com-orkut_117m.graph</li>

 <li>oc-pokec_30m.graph</li>

 <li>rmat_200m.graph</li>

 <li>soc-livejournal1_68m.graph</li>

</ul>

Some useful synthetic, but large graphs include:

<ul>

 <li>random_500m.graph</li>

 <li>rmat_200m.graph</li>

</ul>

There are also some very small graphs for testing. If you look in the <code>/tools</code> directory of the starter code, you’ll notice a useful program called <code>graphTools.cpp</code> that can be used to make your own graphs as well.

By default, the <code>pr</code> program runs your page rank algorithm with an increasing number of threads (so you can assess speedup trends). However, since runtimes at low core counts can be long, you can explicitly specify the number of threads to only run your code under a single configuration.

<pre><code>./pr %GRAPH_FILENAME% 8</code></pre>

Your code should handle cases where there are no outgoing edges by distributing the probability mass on such vertices evenly among all the vertices in the graph. That is, your code should work as if there were edges from such a node to every node in the graph (including itself). The comments in the starter code describe how to handle this case.

You can also run our grader via: <code>./pr_grader &lt;PATH_TO_GRAPH_DIRECTORY&gt;</code>, which reports the correctness of your program and a performance score for four specific graphs.

<strong>You are highly recommended to read the paper, <a href="https://parlab.eecs.berkeley.edu/sites/all/parlab/files/main.pdf" target="_blank" rel="noopener">“Direction-Optimizing Breadth-First Search”</a>, which proposed a hybrid method combining top-down and bottom-up algorithms for breadth-first search, before you continue the following sections.</strong> In Sections <a href="#23-task-2-parallel-breadth-first-search-top-down">2.3</a>, <a href="#24-task-3-bottom-up-bfs">2.4</a>, <a href="#25-task-4-hybrid-bfs">2.5</a>, you will need to finish a top-down, bottom-up, and hybrid method, respectively.

<h3 id="23task2parallelbreadthfirstsearchtopdown">2.3 Task 2: Parallel Breadth-First Search (“Top Down”)</h3>

Breadth-first search (BFS) is a common algorithm that you’ve almost certainly seen in a prior algorithms class.

Please familiarize yourself with the function <code>bfs_top_down()</code> in <code>breadth_first_search/bfs.cpp</code>, which contains a sequential implementation of BFS. The code uses BFS to compute the distance to vertex 0 for all vertices in the graph. You may wish to familiarize yourself with the graph structure defined in <code>common/graph.h</code> as well as the simple array data structure <code>vertex_set</code> (<code>breadth_first_search/bfs.h</code>), which is an array of vertices used to represent the current frontier of BFS.

You can run bfs using:

<pre><code>./bfs &lt;PATH_TO_GRAPHS_DIRECTORY&gt;/rmat_200m.graph</code></pre>

(as with page rank, <code>bfs</code>‘s first argument is a graph file, and an optional second argument is the number of threads.)

When you run <code>bfs</code>, you’ll see the execution time and the frontier size printed for each step in the algorithm. Correctness will pass for the top-down version (since we’ve given you a correct sequential implementation), but it will be slow. (Note that <code>bfs</code> will report failures for a “bottom-up” and “hybrid” versions of the algorithm, which you will implement later in this assignment.)

In this part of the assignment your job is to parallelize a top-down BFS. As with page rank, you’ll need to focus on identifying parallelism, as well as inserting the appropriate synchronization to ensure correctness. We wish to remind you that you <strong>should not</strong> expect to achieve near-perfect speedups on this problem (we’ll leave it to you to think about why!).

<strong>Tips/Hints:</strong>

<ul>

 <li>Always start by considering what work can be done in parallel.</li>

 <li>Some part of the computation may need to be synchronized, for example, by wrapping the appropriate code within a critical region using <code>#pragma omp critical</code>. However, in this problem you can get by with a single atomic operation called <code>compare and swap</code>. You can read about <a href="https://gcc.gnu.org/onlinedocs/gcc-4.1.2/gcc/Atomic-Builtins.html" target="_blank" rel="noopener">GCC’s implementation of compare and swap</a>, which is exposed to C code as the function <code>__sync_bool_compare_and_swap</code>. If you can figure out how to use compare-and-swap for this problem, you will achieve much higher performance than using a critical region.</li>

 <li>Are there conditions where it is possible to avoid using <code>compare_and_swap</code>? In other words, when you <em>know</em> in advance that the comparison will fail?</li>

 <li>There is a preprocessor macro <code>VERBOSE</code> to make it easy to disable useful print per-step timings in your solution (see the top of <code>breadth_first_search/bfs.cpp</code>). In general, these printfs occur infrequently enough (only once per BFS step) that they do not notably impact performance, but if you want to disable the printfs during timing, you can use this <code>#define</code> as a convenience.</li>

</ul>

<h3 id="24task3bottomupbfs">2.4 Task 3: “Bottom-Up” BFS</h3>

Think about what behavior might cause a performance problem in the BFS implementation from Part <a href="#23-task-2-parallel-breadth-first-search-top-down">2.3</a>. An alternative implementation of a breadth-first search step may be more efficient in these situations. Instead of iterating over all vertices in the frontier and marking all vertices adjacent to the frontier, it is possible to implement BFS by having <em>each vertex check whether it should be added to the frontier!</em> Basic pseudocode for the algorithm is as follows:

<pre><code class="c++ language-c++">for(each vertex v in graph)    if(v has not been visited &amp;&amp;        v shares an incoming edge with a vertex u on the frontier)            add vertex v to frontier;</code></pre>

This algorithm is sometimes referred to as a “bottom-up” implementation of BFS, since each vertex looks “up the BFS tree” to find its ancestor. (As opposed to being found by its ancestor in a “top-down” fashion, as was done in Part <a href="#23-task-2-parallel-breadth-first-search-top-down">2.3</a>.)

Please implement a bottom-up BFS to compute the shortest path to all the vertices in the graph from the root (see <code>bfs_bottom_up()</code> in <code>breadth_first_search/bfs.cpp</code>). Start by implementing a simple sequential version. Then parallelize your implementation.

<strong>Tips/Hints:</strong>

<ul>

 <li>It may be useful to think about how you represent the set of unvisited nodes. Do the top-down and bottom-up versions of the code lend themselves to different implementations?</li>

 <li>How do the synchronization requirements of the bottom-up BFS change?</li>

</ul>

<h3 id="25task4hybridbfs">2.5 Task 4: Hybrid BFS</h3>

Notice that in some steps of the BFS, the “bottom-up” BFS is significantly faster than the top-down version. In other steps, the top-down version is significantly faster. This suggests a major performance improvement in your implementation, if <strong>you could dynamically choose between your “top-down” and “bottom-up” formulations based on the size of the frontier or other properties of the graph!</strong> If you want a solution competitive with the reference one, your implementation will likely have to implement this dynamic optimization. Please provide your solution in <code>bfs_hybrid()</code> in <code>breadth_first_search/bfs.cpp</code>.

<strong>Tips/Hints:</strong>

<ul>

 <li>If you used different representations of the frontier in Parts <a href="#23-task-2-parallel-breadth-first-search-top-down">2.3</a> and <a href="#24-task-3-bottom-up-bfs">2.4</a>, you may have to convert between these representations in the hybrid solution. How might you efficiently convert between them? Is there an overhead in doing so?</li>

</ul>

<h2 id="3anamerequirementsrequirementsa">3. <a name="requirements"></a>Requirements</h2>

<h3 id="31part1requirements">3.1 Part 1 Requirements</h3>

You will modify only <code>cg_impl.c</code> to improve the performance of the CG program, such as inserting OpenMP pragmas to parallelize parts of the program. Of course, you may add any other variables or functions if necessary.

<h3 id="32part2requirements">3.2 Part 2 Requirements</h3>