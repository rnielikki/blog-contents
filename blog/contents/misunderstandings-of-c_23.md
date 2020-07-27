[C#]
# Misunderstandings of C#
## C# is copy of Java
C# was sometimes called as "C-like Object Oriented Language" (COOL). C# is just class-based language that looks like somewhat C++. Of course, C# and Java have common features as class-based language, also it’s true that C# is based on Java, but it’s not only based on Java – Sometimes the syntax or feature look more like C++ than Java. For example, Java class methods are virtual as default, C# isn't (Of course non-virtual method can be overridden with "new" keyword, which may look weird). C# has struct. C# can define operators for calculation, which Java cannot but C++ can. C# has unsigned values, pointers in unsafe, and more... But it doesn’t mean C# is copy of C++. C# isn’t just copy of something.
C# is more modern language than Java - which means, Kotlin will be better comparison.

![Image Alt](contents/files/dotnet-neutral.jpg)

*[Image source](https://twitter.com/cjbush/status/1260319306488971276) (removed)*

## C# is non-standard language
After C# 5 with ECMA 334, there’re no official standard for C#. Due to this, everyone thinks C# is non-standard. But there was no need to declare standard of every version, because the feature has been incremental after then. And see also .NET Standard, which contains C# runtime and reflects ECMA 335.

## C# works on only Windows
This is related to .NET. If you mean .NET framework, it’s true. But from 2016, this prejudge is not true, because there are .NET Core. But remember that one technology still only works on Windows – Window GUI (WPF and Windows Forms). If you want to develop them, you can look around of Avalonia, or just can use Blazor PWA (Progressive Web App).
There are no Visual Studio for Linux, however, other IDE supports on Linux, Visual Studio Code, for example.

## C# is .NET
C# is programming language. .NET is not a programming language - it's framework (Of course .NET Core is framework too, which is planned to be combined with .NET Framework). Several language works on .NET : C#, F# and VB.NET. .NET contains runtime for them - CLR (Note that it's **not** C# Language Runtime, but Common Language Runtime).