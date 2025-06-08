---
layout: post
title:  "Embedding JavaScript into C# with Rhino and IKVM"
date:   2009-11-11
categories: C# Rhino IKVM softwaredevelopment
---
The web is full of various discussions on how to embed C# into JavaScript. Most of these approaches are flawed because they rely on the deprecated Microsoft.JScript APIs. Other approaches, like hosting JavaScript in a WebBrowser control, aren't portable. In my particular situation, I need an embedded JavaScript engine that will work on Windows, Mac, and Linux. It has to work equally well in the .NET and Mono runtimes, and ideally, shouldn't require recompiling for each Operating System. I ended up using the IKVM tool to convert Rhino, a JavaScript interpreter written in Java, into a CLR DLL.

["Embedding JavaScript into C# with Rhino and IKVM" on codeproject](https://www.codeproject.com/Articles/41792/Embedding-JavaScript-into-C-with-Rhino-and-IKVM)
