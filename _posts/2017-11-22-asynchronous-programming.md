---
layout: post
title: Asynchronous Programming
---

# Asynchronous Programming
## Why asynchronous ?
We can broadly categorise thread based operations as being either cpu bound or IO bound. Performing prime number calculations would be a classic example of a CPU bound operation whereas pulling down a web page would be a classic example of an IO bound operation.  IO bound operations can be handled in one of two ways. The thread carrying out the IO can block waiting for the results to come back or it can pass in a callback to be executed once the IO completes.  The second approach is the one taken by the new asynchronous API.

Blocking the calling thread is somewhat wasteful. Threads take up memory (1MB per thread) and also cause bookkeeping overhead for the CLR.  In heavy IO bound applications blocking calling threads while waiting for IO is inefficient and reduces throughput. Using asynchronous operations can drastically improve performance. At the operating system level IO is inherently asynchronous because device drivers do the work and the OS only need be involved at start and end of operation. 

> Threads are expensive resources so we want to keep them free. In an ideal world a thread would block only when there is nothing to do. Blocking when hanging around for I/O reduces performance.   

Consider the following code. While it does not block the UI thread, leaving it responsive, it does tie up a thread pool thread which is hanging around doing nothing until the I/O completed

```csharp
        private void GetResultBlocking()
        {
            Task.Run(() =>
                {
                    HttpClient client = new HttpClient();
                    return client
                        .GetStringAsync(_uri)
                        .Result;
                })
                .ContinueWith(task =>
                {
                    TextBox.Text = task.Result;
                }, TaskScheduler.FromCurrentSynchronizationContext());
        }
```

We can improve this code using the asynchronous approach as follows.  Now the calling thread will not block and will instead pass a continuation to be invoked when the call returns.

```csharp
        private void GetResultAsync()
        {
            HttpClient client = new HttpClient();
            Task<string> stringAsync = client
                .GetStringAsync(_uri);
            stringAsync.ContinueWith(task =>
            {
                TextBox.Text = task.Result;
            }, TaskScheduler.FromCurrentSynchronizationContext());
        }
```

If we use the async await compiler support added in C# 5.0 then our code becomes even nicer

```csharp
        private async void GetResultAsyncAwait()
        {
            HttpClient client = new HttpClient();
            Task<string> tsk = client
                .GetStringAsync(_uri);
            var res = await tsk;
            TextBox.Text = res;
       }
```


 #csharp/threads/asyncawait
