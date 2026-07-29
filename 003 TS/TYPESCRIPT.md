
JS is single threaded non blocking asynchronous concurrent language

single threaded = one thread = one call stack = one thing at a time (JS Runtime)


for example setTimeout doesnt live in v8 source. its extra stuff.




STACK does its regular job, inside v8 engine
webapis -> task queue (when job is done, pushes callback to task queue)

setTimeout(
    function callback() {
        console.log("hello");
    }, 5000);

event loop --> if stack is empty, takes first thing on task queue and pushes to stack
so settimeout is actually minimum time for execution, not exact