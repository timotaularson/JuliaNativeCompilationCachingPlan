**Julia Native Compilation Caching Plan**  

Motivating Idea:

- The standard way of using Julia would benefit from native code caching
- Other than reduced latency this should not change how you interact with the system

Approach:

- Compile a shared library to lifecycle with each ji file including
    - a format version, because implementations usually change over time    
    - a linking table    
    - list of external references by signature    
        - runtime linker resolves each to a live address for the linking table           
    - signature list of included native functions
        - runtime linker translates each to an address to put in a _jl_code_instance_t                                 
    - the native compiled functions    
        - compiled such that external references are indirect via the linking table               
    - signatures will probably need to include some form of relevant world age ranges

Questions:

- What needs changed in the above proposal?
- How do the above pieces need to interact with the garbage collector?
- How can we collect the necessary signatures during the compilation process?
- How do the signatures need to be serialized to the shared library?
- How can we resolve the signatures to live addresses from the running system?
- How can we cause the native compilation to use indirection through a linking table?
