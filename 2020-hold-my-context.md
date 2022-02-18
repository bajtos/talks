# Hold my context!

☝️  _the presentation title_☝️

## Short summary for display

Preserving contextual data across asynchronous calls has been always tricky
in Node.js land. For example, how to pass information about the current user
or a correlation (transaction) id from the code processing incoming HTTP
requests to the code calling a backend web service? Traditional runtimes like
C and Java can utilize thread-local storage, which does not work for Node's
single-threaded event loop.

The talk will describe different solutions people have been using (and
abandoning) over the past 6+ years, dive into the history of domains and async
hooks, explain what's so difficult about context keeping. Finally, I'll discuss
the `AsyncLocalStorage` API introduced by Node.js 14 and show how to use it in 
practice.

## Talk description and abstract

_Structure, table of contents and main content points._

1. Why would anyone need to preserve a context? Start with an example explaining
  the problem faced by server developers: how to forward correlation-id from
  an incoming request to a backend web service. Show how easy this is in the sync
  world (C/Java/.NET).

2. Solution 1: core `domains`. While they can track async transitions and
  continuations, they are not meant to store custom data.

3. Solution 2: `continuation-local-storage`. The go-to solution during Node.js
   0.10 days. Unfortunately, it often drops the context and requires
   applications to cooperate, which often means fixing dependencies 😱.

4. Explain why it is difficult to preserve the context (user-land promises,
   connection pool problem). Talk about our own experience with issues caused
   by the MongoDB driver. Mention other use cases affected by this problem
   (APM tools like NewRelic).

5. Solution 3: explicit context passing in function arguments. Very reliable
   if you remember to always pass the context in function calls, but also
   rather tedious.

6. Introduce the idea of `async_hooks`, talk a bit about the history of the
   implementation (`AsyncListener`, `async_wrap`).

7. Solution 4: `cls-hooked` (Node.js 4.7+).  Works reasonably well, but
   depends on experimental/undocumented APIs and requires careful order of
   initialization (loading of dependencies).

8. While `async_hooks` provide the required building blocks, the API is too
   low-level. We need a better solution! Introduce `executionAsyncResource`
   and `AsyncLocalStorage` APIs.

9. Using the example presented at the beginning, show how to use `AsyncLocalStorage`
   API to implement forwarding of correlation-id values from incoming request
   headers to the outgoing request headers when calling a web service.

10. Conclusion. Availability of `AsyncLocalStorage` API, known limitations, etc.
