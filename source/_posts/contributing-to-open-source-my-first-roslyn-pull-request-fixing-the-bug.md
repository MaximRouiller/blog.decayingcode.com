---
title : "Contributing to Open-Source - My first roslyn pull request - Fixing the bug"
date: 2017-04-04 13:30
tags: [open source, c#,  .net, visual studio]
---

### Getting your environment ready

If you haven't done so yet, please check [part 1](/post/contributing-to-open-source-my-first-roslyn-pull-request-getting-the-environment-ready/) to get your environment ready. Otherwise, you'll just run in circle.

### Reading the bug

After getting the environment ready, I decided to actually read what the bug was all about. I did a quick glance at first but I'll be honest... I was hooked by  the "low hanging fruit" description and the "up for grabs" tag. I didn't even understand was I was throwing myself into...

Here's the bug: [dotnet/roslyn/issues/18391](https://github.com/dotnet/roslyn/issues/18391).

So let's read what it is.

![The Bug](/posts/files/first-roslyn-pr/the-bug-part-1.png)

So basically, there's a refactoring when you are hiding a base member to put the `new` keyword on the derived property.

That refactoring was broken for fields.

It refactored to:
```csharp
public class DerivedClass : BaseClass {
    public const new int PRIORITY = BaseClass.PRIORITY + 100;
}
```

Instead of:
```csharp
public class DerivedClass : BaseClass {
    public new const int PRIORITY = BaseClass.PRIORITY + 100;
}
```

Do you see it? The order of the parameters are wrong. The `new` keyword **MUST** be before the `const` declaration. So the refactoring causes a compilation error. Hummm... that's bad.

### Help. Please?

That's where Sam really helped me. It's how it should be in any open source project. Sam [pointed me in the right direction](https://github.com/dotnet/roslyn/issues/18391#issuecomment-291164250) while I was trying to understand the code and find a fix.

### The problematic code

[HideBaseCodeFixProvider.AddNewKeywordAction.cs](https://github.com/MaximRouiller/roslyn/blob/2594c27aa91fa73c44ecefd7cd23552169857c9d/src/Features/CSharp/Portable/CodeFixes/HideBase/HideBaseCodeFixProvider.AddNewKeywordAction.cs)

```csharp
private SyntaxNode GetNewNode(Document document, SyntaxNode node, CancellationToken cancellationToken)
{
    SyntaxNode newNode = null;

    var propertyStatement = node as PropertyDeclarationSyntax;
    if (propertyStatement != null)
    {
        newNode = propertyStatement.AddModifiers(SyntaxFactory.Token(SyntaxKind.NewKeyword)) as SyntaxNode;
    }

    var methodStatement = node as MethodDeclarationSyntax;
    if (methodStatement != null)
    {
        newNode = methodStatement.AddModifiers(SyntaxFactory.Token(SyntaxKind.NewKeyword));
    }

    var fieldDeclaration = node as FieldDeclarationSyntax;
    if (fieldDeclaration != null)
    {
        newNode = fieldDeclaration.AddModifiers(SyntaxFactory.Token(SyntaxKind.NewKeyword));
    }

    //Make sure we preserve any trivia from the original node
    newNode = newNode.WithTriviaFrom(node);

    return newNode.WithAdditionalAnnotations(Formatter.Annotation);
}
```

We are not interested in the first 2 parts (propertyStatement, methodStatement) but in the 3rd one. My first impression was that `const` was considered a Modifier and that `new` was also a modifier. The normal behavior or `List.Add(...)` is to append at the end (`const new`).

That's where the bug is.

### The Solution

Well... I'm not going to walk you through the whole process but basically, I tried to do the job myself by trying to insert the `new` keyword at Position 0 in an array. After a few back and forth with [Cyrus Najmabadi](https://github.com/CyrusNajmabadi), we finished on a syntax that should use their new `SyntaxGenerator`.

The `SyntaxGenerator` will ensure that all modifiers are in the proper order. Also, it works for all properties. Here is the refactored code once all the back and forth were done.

```csharp
private SyntaxNode GetNewNode(Document document, SyntaxNode node, CancellationToken cancellationToken)
{
    var generator = SyntaxGenerator.GetGenerator(_document);
    return generator.WithModifiers(node, generator.GetModifiers(node).WithIsNew(true));
}
```

Wow. Ain't that pretty.

### Unit Testing

[HideBaseTests.cs](https://github.com/MaximRouiller/roslyn/blob/b16c100bde7e4408dfae3961d139a7309221dcf4/src/EditorFeatures/CSharpTest/Diagnostics/HideBase/HideBaseTests.cs)

A big part of Roslyn are the tests. You can't write a compiler and have zero tests. There's enough test in there to melt a computer.

The biggest amount of back and forth we did were on the tests. In fact, I added more lines of tests in this than actual production code.

Here's a sample test I did:

```c
[WorkItem(14455, "https://github.com/dotnet/roslyn/issues/14455")]
[Fact, Trait(Traits.Feature, Traits.Features.CodeActionsAddNew)]
public async Task TestAddNewToConstantInternalFields()
{
    await TestInRegularAndScriptAsync(
@"class A { internal const int i = 0; }
class B : A { [|internal const int i = 1;|] }
",
@"class A { internal const int i = 0; }
class B : A { internal new const int i = 1; }
");
}

```

First, they are using xUnit so you have to brand everything with `FactAttribute` and `TraitAttribute` to properly category the tests.

Second, if you add a test that fixes a GitHub issue, you have to add the `WorkItemAttribute` the way I did it in this code. If you do this, Your test work.

Finally, the `[| ... |]` syntax. I don't know anything about it but my assumption at the time was that it was what we were going to *refactor* and how we were expecting it to be handled.

### Testing in Visual Studio

Remember when we installed the Visual Studio Extensibility Workload?

That's where we use it. In the `Roslyn.sln` opened solution, find the project `VisualStudioSetup` and make it your start-up project. Hit F5.

A new instance of Visual Studio will be launched and you will be able to test your newly updated code. Launch a separate instance of Visual Studio.

You now have 1 experimental Visual Studio with your fixes and 1 standard instance of Visual Studio without your fixes.

You can now write the problematic code in the standard instance and see the problem. Copy/paste your original code in the experimental instance and marvel at the beauty of the bug fix you just created.

### Creating the pull request

As soon as my code was committed and pushed to my fork, I only had to create a pull request from the GitHub interface. Once that pull request is created, any commits that you create on top of your fork will be included in this pull request.

This is where the back and forth with the .NET Team started truly.

### What I learned

Building the compiler isn't easy. Roslyn is a massive 200 projects solution that takes a lot of time to open. Yes. Even on the Surface Book pimped to the max with an SSD, an i7 and 16GB of RAM. I would love to see a better compartmentalization of projects so I don't open those 200 projects all at once just to build a UI fix.

Sam and I had to do a roundtrip on the `WorkItemAttribute`. It wasn't clear how it should be handled. So he created [a pull request](https://github.com/dotnet/roslyn/pull/18403) to address that.

Talking about tests, the *piped string* notation was really foreign to me and took me a while to understand. Better documentation should be among the priorities.

### My experience

I really loved fixing that bug. It was hard, then easy, then lots of back and forth.

In the end, I was pointed in the right direction and I really want to tackle another one.

So my thanks to Sam Harwell and Cyrus Najmabadi for carrying me through fixing a refactoring bug in Visual Studio/Roslyn. If you want contributors, you need people like them to manage your open source project.

### Will you contribute?

If you are interested, there is [a ton](https://github.com/dotnet/roslyn/issues?q=is%3Aopen+is%3Aissue+label%3Aup-for-grabs) of issues that are `up-for-grabs` on the roslyn repository. Get ready. Take one and help make the Roslyn Compiler even better for everybody.

If you are going to contribute, please let me know in the comment! If you need more details, please also let me know in the comments.
