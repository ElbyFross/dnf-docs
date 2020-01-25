\page FAQ

# Welcome to the F.A.Q. block
I hope here you find answers for your questions. In case if some common questions not covered don't be shy to contact me via [LinkedIn](https://www.linkedin.com/in/podshyvalov/).

## Remarks
* I have receiving several dozens emails each day. Be patient with waiting for response.
* Please don't ask about cases relative only for your project. I am answering only on questions that could be faced by common developer group. 
* Sure I can consult you directly about your product but that's will require signing up a custom contract or providing a valuable sponsorship to the DNF development. 
Every case considering separately and price depends from complexity of the task and scale of the product.

***

#### Q: Can't connect remote computer.
**A:** Be sure that server has:
- oppened **445** port.
- **LSA** policy allows network access.
- security rules allows **anonymous access** in case of logon without defined [LogonConfig](https://github.com/ElbyFross/doloro-networking-framework/wiki/Instructions#logon-config) params. In that case app will impersonated as anonymous identity (SID NULL)
- in case of connection as guest be sure that **Guest user enabled** in the **LSA** and user's rights **not restrict** remote logon as guest (*restricted by default*).

***

#### Q: "Incorrect user name or password" message during conenction
**A:** Maybe you trying to logon as anonymous user and that restricted by rules. Otherwise that means that a [LogonConfig](https://github.com/ElbyFross/doloro-networking-framework/wiki/Instructions#logon-config) defined at your [RoutingTable](https://github.com/ElbyFross/doloro-networking-framework/wiki/RoutingTable) not filled or has incorrect logon params.

***

#### Q: What an encryption algorithms using during transmission?
**A:** By default DNF using RSA for query's metadata encryption and AES for content encryption.

***

#### Q: Could I implement my own encryption algorithms?
**A:** Sure. Look at that [acrticle](https://github.com/ElbyFross/doloro-networking-framework/wiki/Custom-transmission-encryptor).

***

#### Q: Can I relay transmission through a server?
**A:** The DNF has implemented [RelayServer](https://github.com/ElbyFross/doloro-networking-framework/wiki/RelayServer) like a base entity for such operations. You may look at the [QueriesServer](https://github.com/ElbyFross/doloro-networking-framework/wiki/QueriesServer) to find out how use it at real project.

***

#### Q: Can I use framework at local apps ecosystem?
**A:** You can and it will excellent solution. The DNF using a named pipes for transmission. That a one of the best way to cross process data exchanging at WinNT. So you can and it will work fine.

***

#### Q: I equeued a bunch of queries and seems like a first one is stuck. Why and what sould I do?
**A:** Queries could stop enqueue execution in few cases. 
- You enqueued a duplex query and setted `Query.WaitForAnswer` property to `true`, and server still not sent an answer back to the client. 
- Connection to server not established or server not exist.

You can interrupt the query execution by setting `true` to the `TransmissionLine.Interrupted` property. At the next client loop step handler will terminated. Before that you could consider to re enqueue the query using the line like `line.EnqueueQuery(line.LastQuery);`.

***
#### Q: I am using an encrypted transmission but I need to send a query to the server without encryption. How I can do that?
**A:** Just create a query by the way `new Query(false, .. YOUR QUERY PARTS...)`. With first `false` parameter you disabling encryption of that query.

If you want to force sending not encrypted answers from server from some [IQueryHandler](https://github.com/ElbyFross/doloro-networking-framework/wiki/IQueryHandler) then just take care about removing the `pk` parameter from the encry query before opennig a backward transmission.

***
#### Q: My server must handle some specific query. How to implement it?
**A:** Queries handling at the server side implementing by using the [IQueryHandler](https://github.com/ElbyFross/doloro-networking-framework/wiki/IQueryHandler).