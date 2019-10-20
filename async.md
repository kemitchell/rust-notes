- close to Rust standard library
- filesystem
- network
- concurrency basics
- `task` similar to Rust `thread`
- `async`/`await`-compatible `Mutex` &c.
- "Future"
  1.  `std::future::Future`
  2.  `futures::future::Future` from futures-rs crate
  - (2) came first
  - (2) moved into core to enable `async`/`await`
  - Don't interact with `std::future::Future`, other than to call `.await`.
  - async-std reexports core `Future`
  - async-std implements all futures traits for its types.
- `async_std::future` support functions
- streams, Read, Write, Seek, BufRead implemented in `async_std`, but can't be implemented by users.
- Futures concepts
  1.  deferred computation
  2.  asynchronicity
  3.  independence of execution strategy
- executor execute futures
- `async_std::task` interface
- "fearless concurrency"
- `Send` and `Sync` abstract over concurrency strategies
  - `Send`: passing data to another computation
    - sender loses access
    - received gains access
  - `Sync`: sharing data between concurrent computations
    - e.g. mutex
    - e.g. spinlock
- simple computation
  1.  sequence of composable operations
  2.  can branch
  3.  either yield a result or yield an error
- Future:  Start doing X.  Once X succeeds, start doing Y.
- The word "something" implies a trait in Rust.
- `poll`:
  - done, `Poll::Ready`
  - ongoing, `Poll::Pending`
- `async_fn`
- `.await`
- "runtime"
- `tasks` module runs futures.
- `block_on(some_task);`
- `async {...}`
- `task::spawn(async {...});`
- "cold future": need to start them
- `JoinHandles` are futures
- tasks
  - attributes
    - single allocation
    - backchannel for communications via `JoinHandle`
    - debugging metadata
    - task-local storage
  - run concurrency
  - may share a thread
  - OS thread blocking operations block all tasks on the same thread.
- "fallible": `Result<T,E>`
