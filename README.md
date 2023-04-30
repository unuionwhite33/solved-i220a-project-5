Download Link: https://assignmentchef.com/product/solved-i220a-project-5
<br>
The aims of this project are as follows:

<ul>

 <li>To allow you to simulate a read-only cache.</li>

 <li>To expose you to typical random number generation.</li>

</ul>

<h1>1.2        Requirements</h1>

Implement a module which implements a simulator for a read-only cache. Specifically, implement the following ADT:

/** Opaque implementation */ typedef struct CacheSimImpl CacheSim;

/** Replacement strategy */ typedef enum {

LRU_R,                       /** Least Recently Used */

MRU_R,                       /** Most Recently Used */

RANDOM_R                /** Random replacement */

} Replacement;

/** A primary memory address */ typedef unsigned long MemAddr;

/** Parameters which specify a cache.

*            Must have nMemAddrBits <em>&gt; </em>nLineBits <em>&gt;</em>= 2.

*/ typedef struct {

unsigned nSetBits; /** Text notation: s; # of sets is 2**this */ unsigned nLinesPerSet; /** Text notation: E; # of cache lines/set */ unsigned nLineBits; /** Text notation: b; # of bytes/line is 2**this

*/ unsigned nMemAddrBits;             /** Text notation: m; # of bits in primary mem addr; total primary addr space is 2**this */

Replacement replacement; /** replacement strategy */

} CacheParams;

/** Create and return a new cache-simulation structure for a *               cache for main memory with the speci ed cache parameters params.

*          No guarantee that *params is valid after this call.

*/

CacheSim *new_cache_sim(const CacheParams *params);

/** Free all resources used by cache-simulation structure *cache */ void free_cache_sim(CacheSim *cache);

typedef enum {

CACHE_HIT,                                             /** address found in cache */

CACHE_MISS_WITHOUT_REPLACE, /** address not found, no line replaced */

CACHE_MISS_WITH_REPLACE,           /** address not found in cache, line replaced

*/

CACHE_N_STATUS                                   /** # of status values possible */

} CacheStatus;

typedef struct {

CacheStatus status;           /** status of requested address */

MemAddr replaceAddr; /** address of replaced line if status is

*             CACHE_MISS_WITH_REPLACE */

} CacheResult;

/** Return result for addr requested from cache */

CacheResult cache_sim_result(CacheSim *cache, MemAddr addr);

Submit a submit/prj5-sol directory in your github repository such that typing make within that directory will build a cache-sim executable program. The program must start execution at the main program provided (see below) which drives your implementation of the above API.

To further clarify what must be returned by the cache_sim_result() function:

The status should be set to

CACHE_HIT The data speci ed by the address is present in the cache

CACHE_MISS_WITHOUT_REPLACE The speci ed data is not present in the cache but when the data was read from main memory into the cache it was not necessary to replace any cache line because a free (invalid) cache line was available.

CACHE_MISS_WITH_REPLACE The speci ed data is not present in the cache and there was no free cache line so that when the data was read from main memory into the cache it was necessary to replace valid data stored in the cache.

In the last case, the return value must include replaceAddr which should be set to the address of the block which was replaced; i.e. replaceAddr must include the tag and set bits of the replaced address with the block o set bits set to 0.

<h1>1.3         The main Program</h1>

You are being provided with a main program to exercise your implementation of the above simulation API. When this program is run, it expects a required argument of the form s-E-b-m where:

s The number of address bits used to specify a cache set; the cache will contain a total of 2<em><sup>s </sup></em>sets.

E The number of cache lines stored in each cache set; b The number of address bits used to specify an o set within a cache line (block). The size of a cache block will be 2<em><sup>b </sup></em>bytes.

m The total number of address bits used to specify a primary memory address. The total size of the main memory will be 2<em><sup>m </sup></em>bytes.

Optionally, the main program accepts one or more of the following options before the above required argument:

<ul>

 <li>REPLACEMENT The replacement strategy to be used by the cache simulator. Possible values are lru (for least recently used), mru (for most recently used) or rand (for random replacement). The default is lru.</li>

 <li>SEED SEED must be non-negative integer specifying a seed for the random number generator. The default value is 0.</li>

</ul>

-v If this option is speci ed, then the program produces additional verbose output.

You should ensure that when the program is invoked, the $HOME/cs220/lib directory will be searched for necessary libraries.

When run, the main program is set up to read whitespace-separated hexadecimal addresses from standard input and repeatedly call your cache_sim_result(¬ ) function with those addresses. If the -v verbose option is speci ed, then it prints out the result of each call. Otherwise, it will merely print out a summary of the cache statistics when end-of- le is encountered on standard input.

<h1>1.4           Random Number Generation</h1>

Most programming environments provide a pseudo-random number generation facility. The facility is pseudo in that it does not generate real random numbers but only pseudo random numbers in that the generated sequence meets some statistical tests for randomness. In fact, to facilitate testing, the random sequence is repeatable and is controlled by a seed for the random number generation.

In the C programming environment, the random number facilities are available using two library functions rand() and srand() which are speci ed in the &lt;stdlib.h&gt; header le; successive calls to the former function return random non-negative integers; the seed of the random number generator can be speci ed by calling srand(). You will need to use the rand() function to simulate the random cache replacement strategy speci ed by the -r rand option.

<h1>1.5         The lib Directory</h1>

You may continue to use the following library contained within the course lib directory:

libcs220:: A trivial library which provides help with memory allocation and error reporting:

<ul>

 <li>Provides checked versions mallocChk(), reallocChk(), and callocChk¬ () of the memory allocation routines which wrap the standard memory allocation routines with the program exiting on failure. The speci cation le is in h.</li>

 <li>Provides routines for reporting errors using printf()-style format strings with one modi cation: if the format string ends with :, then strerror¬ (errno) is appended to the error-message. The speci cation le is in h.</li>

</ul>

You may assume that the environment in which your program will be compiled and run will have this library available.

<h1>1.6        Provided Files</h1>

The prj5-sol directory contains the following          les:

main.c This provides the main() program described above. You should not modify this le.

Cache Simulator Files The cache-sim.h le contains the speci cation for the API you are required to implement. You should not modify this le.

The cache-sim.c le will be the le where you do your work. As provided, it contains a skeleton implementation su cient to compile the program without errors. You should remove the placeholder return values and esh out the function de nitions given there (adding in any auxiliary declarations or de nitions which may be needed).

Make le A Makefile for the project. Note that this Makefile is set up to include a reference to the $HOME/cs220/lib directory in the executable so that it is possible to run the executable without needing to set up LD_LIBRARY_PATH. You should not need to modify this le.

README             A README template.

You must submit all the les in the above prj5-sol directory whether you modify them or not.

The aux- les directory contains the following            les:

Test les lru_2-2-2-8.test and mru_4-2-8-16.test These les provides addresses which can be used for testing your program. The les contain the expected output as comments following # characters. The options to your program should be those corresponding to the name of the test le. You will need to strip out the # comments before feeding the le to your program; this can be done simply using a Unix utility like sed. For example, assuming that you are in the directory containing your executable:

$ sed -e ’s/#.*//’ $HOME/cs220/projects/prj5/aux-files/lru_2-

2-2-8.test 

| ./cache-sim -r lru -v 2-2-2-8

If your simulator is working correctly, your output should match that shown within the test le.

A ruby script address-trace.rb This script can be used to generate address traces which can be piped into your program. It generates the following kind of traces:

matrix Addresses used to access a square matrix. The size of each matrix element is hardwired to be 8, but you can specify the size of the matrix as well as the access pattern (by row or by column).

program A typical program address trace. You can specify the probability of intra-procedure branches or inter-procedure calls/returns.

random A random address trace.

You can run the script with a –help option in general:

$ ./address-trace.rb –help to get general usage information or on a speci ed subcommand:

$ ./address-trace.rb matrix –help

For example, to run your program with an address trace corresponding to row-access for a 1024×1024 matrix:

$ $HOME/cs220/projects/prj5/aux-files/address-trace.rb matrix 

–access=row 

| ./cache-sim -r lru -v 4-4-8-32 &gt;t.log

To repeat, for column-access:

$ $HOME/cs220/projects/prj5/aux-files/address-trace.rb matrix 

–access=col 

| ./cache-sim -r lru -v 4-4-8-32 &gt;t.log You should not submit les from the aux-files directory.

<h1>1.7       Hints</h1>

Review the material covered in class and from the text pertaining to memory caches. Also, review online or other material to understand how to use the rand() function.

The following steps are merely suggestions:

<ol>

 <li>Look at how you can implement the CacheSimImpl structure for your cache simulator. Note that your cache simulator is not required to track the cached data, merely to simulate the hit/miss behavior of a real cache. Hence it seems su cient to merely:

  <ul>

   <li>For each cached line, merely track the address of the block it contains.</li>

   <li>When the cache is empty, the contents of all cached lines will be invalid. As the cache lls up, these contents will become valid. Hence tracking the valid/invalid status of a cached line will be necessary.</li>

   <li>For implementing the di erent replacement strategies, you will need to gure out how you will track the age of a cached line and/or organize them in a manner which facilitates the replacement strategy.</li>

   <li>Since the cache parameters are only provided to the new_cache_¬ sim() function and you will need those parameters to simulate the cache in cache_sim_result(), the CacheSimImpl structure will also need to track the cache parameters provided to new_cache_sim().</li>

  </ul></li>

 <li>Since the cache parameters are available only at runtime (via the CacheParams argument), you will need to use dynamic memory allocation for the cache simulation.</li>

</ol>

Abstractly, a cache consists of a collection of cache sets with each cache set consisting of a collection of cache lines; i.e. a cache is a collection of collections. Alternatives for representing collections in C include arrays, linked lists, etc.

If you go with using arrays for representing collections, you might wish to use multi-dimensional arrays. OTOH, if you set up ragged arrays (as described in the transparencies), you can still use multi-dimensional array notation with dynamic memory (at the cost of extra memory for pointers).

Speci cally, you can always use dynamic allocation to get a pointer to a vector of elements and then use array indexing notation with the return’d pointer. For example, to emulate int a[m][n] using dynamically allocated ragged arrays, one can do:

int **a = malloc(m * sizeof(int *));

for (int i = 0; i <em>&lt; </em>n; i++) {

a[i] = malloc(n * sizeof(int));

}

//can now use array notation like a[i][j].

<ol start="3">

 <li>Start out your implementation without bothering about the replacement strategy; i.e. simply use any ad-hoc replacement strategy which is convenient for your code.</li>

 <li>Once you have veri ed that your code works for accessing the correct set and cache line, implement the di erent replacement strategies. For each cache line you could track when it was last accessed (where time can be tracked using a simulation clock which is advanced for each step of the simulation). Alternately, you could track relative access age using ordering within the tracked information.</li>

</ol>

Iterate until you meet all requirements.