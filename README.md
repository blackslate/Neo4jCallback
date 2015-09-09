# Neo4jCallback
Demo to show that no callback is made on the server after a Meteor.neo4j.query(...) call

When you run this app, check in the Terminal for output. This is what I see:

    Meteor is successfully connected to Neo4j on http://neo4j:1234@localhost:7474
    callbackTest running on server
    Done
    => Meteor server restarted
    Destroyed:  null []
    Created:  null [ { a: { db: [Object], _request: [Object], _data: [Object] } } ]
    MATCH (a:Node)-->(b:Node) WHERE a.name = {name} RETURN b { name: 'World' }

I'm expecting to see a line like...

    testCallback on server null Object {n: Array[2], count(n): 1}
  
... after "Done", just like there is on the client, as shown here:

    callbackTest running on client
    Done
    testCallback on client null Object {n: Array[2], count(n): 1}
    
The relevant code is in lines 21 - 29 of neo4jCallback.qs

    // The following call is made both on the server and ont the client.
    var environment = (Meteor.isClient) ? "client" : "server"
    console.log("callbackTest running on "+environment)
    var callbackTest = Meteor.neo4j.query('MATCH (n:Node) RETURN n, count(n)', null, testCallback);
    console.log("Done")

    function testCallback(error, data) {
      console.log("testCallback on " + environment, error, data)
    }
