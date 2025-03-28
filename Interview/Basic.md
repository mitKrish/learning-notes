Design patterns used in project
native object & port object
REST API - Stateless
NGnix server
Message Queue
Secure FE
Cluster
middleware chaining


Promise.race(Promise.resolve)

(function () {
    try {
        throw new Error();
    } catch (y) {
        var x = 1, y = 2;
        console.log(x);
    }
    console.log(x);
    console.log(y);
})();

chrome / Node.js - updating window (chrome)/ global object (node)
{
    console.time("loop");
    for (var i = 0; i < 1000000; i += 1){
        // Do nothing
    }
    console.timeEnd("loop");
}
