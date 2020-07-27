[Blazor]
# Testing Backend thing on Blazor
## File I/O

![Image Alt](contents/files/blazor-backend/1.png)

![Image Alt](contents/files/blazor-backend/2.png)

This one results “/” (Always, I tested on ‘/counter’ page too).

I checked storages on browser tool, but nothing was found. Looks like it uses virtual storage on memory – It was emptied every time when the program starts.

## Creating threads

JavaScript doesn’t support multithreading. WebAssembly may support, but not on the browser. So it’s maybe fun to test.
And guess what, multithreading is blocked.

```csharp
var t1 = new System.Threading.Thread(new System.Threading.ThreadStart(()=>Console.WriteLine("first")));
var t2 = new System.Threading.Thread(new System.Threading.ThreadStart(()=>Console.WriteLine("second")));
t1.Start();
t2.Start();
```
![Image Alt](contents/files/blazor-backend/3.png)

## IRCBot
It will be interesting if my old IRCBot works on Blazor. But it needs to open socket. So, does it work?

![Image Alt](contents/files/blazor-backend/4.png)

[TCP Socket opening](https://developer.mozilla.org/en-US/docs/Archive/B2G_OS/API/TCPSocket) is not supported. Even MDN says it works only certain states. The raw TCP socket support has security problem, so W3C [marked it as retired](https://www.w3.org/TR/2015/NOTE-tcp-udp-sockets-20150723/). It’s highly reasonable not to support it.

## Conclusion
Some features should not work on the front-end side (and cannot work), and Blazor did well with this. I mean, maybe WebAssembly did well and Blazor is just implemented version of WebAssembly. (Really, [WebAssembly is more secure](https://webassembly.org/docs/security/) than native C or C++)
