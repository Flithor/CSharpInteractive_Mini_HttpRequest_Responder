# CSharpInteractive_Mini_HttpRequest_Responder

A short C# script code can be used in C# Interactive(VS 2015+)

Genterat a simple http responder for test your http request

Enjoy!

Code for fast copy:
```
//Copy Code and paste in C# Interactive Window(Existed in VS 2015+, "View->Other Window->C# Interactive")
//Modify as you want and enjoy!
//en-us-----------------------
//If you writen an http request and need a response test, but the target API hasn't finished,
//you can use this code to built a simple responder in C# Interactive window,
//without to create a new webapi project for testing!
//zh-cn-----------------------
//如果你写了一个Http请求需要测试,但是对方还没有完成接口的话
//这段代码就可以让你实现自己验证http请求与响应的逻辑
//可以直接在VS2015及以上版本的C# Interactive窗口中运行

using System.Net;
var l = new HttpListener();
l.AuthenticationSchemes = AuthenticationSchemes.Anonymous;
l.Prefixes.Add("http://localhost:8080/API/XXXX/");//ATTACTION: Uri should end with "\"
//more target url

var t = Task.Run(() =>
{
    Print("START!");
    l.Start();
    try
    {
        while (l.IsListening)
        {
            HttpListenerContext ctx = l.GetContext();//here for waiting
            Print(new StreamReader(ctx.Request.InputStream).ReadToEnd());
            ////Set Response here
            //ctx.Response.StatusCode = 200;
            //ctx.Response.OutputStream.Write("true".Select(c => (byte)c).ToArray(), 0, 4);
            ctx.Response.Close();//"Close" for return response
        }
    }
    catch { Print("STOP!"); }
});

//l.Stop(); //Call this method to stop HttpListener
```
