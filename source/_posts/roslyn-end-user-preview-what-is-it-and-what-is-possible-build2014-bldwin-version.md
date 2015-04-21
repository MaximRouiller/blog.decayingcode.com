---
title: "Roslyn End-User Preview – What is it and what is possible? – #build2014 #bldwin version"
date: 2014-04-03 14:40:00
tags: [build,tool]
---

### What is Roslyn?

Roslyn is a complete new compiler for .NET. However, it's more than just a simple compiler. We called it earlier a &ldquo;compiler as a service&rdquo;. Now they call it the compiler platform.

### What's new?

Well it ain't your run of the mill compiler. It doesn't just take code and outputs machine code (or IL for .NET).

This compiler allows you to participate in the compilation of your software itself and tell the compiler what to do with it. Scenarios like Aspect Oriented Programming becomes relatively trivial and doesn't require the use of plugins or post-build event.

You have a cool refactoring that you want to implement in a very specific way? Want to convert properties with certain attributes to code blocks? Just code it. Roslyn allows you to integrate your refactoring within Visual Studio directly and share it with everyone. One specific scenario would be to code company code guidelines directly within a VSIX that you deploy on every developer's machine. This allow developers to all have the same rules as for what the company is concerned. This could definitely give an edge to a company that want to standardise code quality directly at the source.

### APIs

Basically, it comes with three type of API. Features, workspaces (solution, projects, files) as well as the compiler APIs.

Features are based around refactoring and fixing code. Those are high level functionality that are tightly link to Visual Studio. Workspace API relate to code formatting, finding references, etc. They are also linked to Visual Studio. Compiler API relates to Syntax trees, emitting code, analysing flows of code&hellip; they are the most low-level API related to the compiler and are also the most interesting.

### What's coming?

Well... technically you now have access to the C# compiler with an Apache 2 licence.

Here's what is now possible&hellip;

*   [Participate in the next features of C#](https://roslyn.codeplex.com/wikipage?title=CSharp%20Language%20Design%20Notes&amp;referringTitle=Documentation "CSharp%20Language%20Design%20Notes&amp;referringTitle=Documentation")
*   Create a new language and integrate it within Roslyn
*   Better/custom code analysis for your code
*   Creating compiled .NET DSL for businesses

### Simple scenario #1 &ndash; Creating a new refactoring

Using the Roslyn SDK, I create a new Visual C# > Roslyn > Code Refactoring.

The default template reverse the string of a type. So I press F5, create a new project, create a class and do ALT + . on that class.

I now have an additional refactoring option which brings my class &ldquo;ThisTest&rdquo; to &ldquo;tseTsihT&rdquo; with live preview. This is nothing but it's a refactoring that you are 100% in control with which doesn't require external tools.

> I know. This refactoring is useless. If something is valuable for you, you can simply implement it or wait for someone in the community to develop it.

### Simple Scenario #2 &ndash; Flagging improperly named fields

So let's say we want to flag any field that uses the old &ldquo;m_something&rdquo; convention. Doing this is as simple as the following code:

```cs
/// <summary>
/// This is used to identify where problems are.
/// </summary>
[DiagnosticAnalyzer]
[ExportDiagnosticAnalyzer("NoMUnderscore", LanguageNames.CSharp)]
internal class FieldsDoNotStartWithMUnderscore : ISyntaxNodeAnalyzer<SyntaxKind>
{
    public const string RemoveMDiagnosticId = "NoMUnderscore";
    public static readonly DiagnosticDescriptor RemoveMUnderscoreRule = new DiagnosticDescriptor(RemoveMDiagnosticId,
                                                                                         "Remove m_",
                                                                                         "Invalid name. Field name must not start with m_",
                                                                                         "Usage",
                                                                                         DiagnosticSeverity.Error);

    public ImmutableArray<SyntaxKind> SyntaxKindsOfInterest { get { return ImmutableArray.Create(SyntaxKind.FieldDeclaration); } }

    public ImmutableArray<DiagnosticDescriptor> SupportedDiagnostics { get { return ImmutableArray.Create(RemoveMUnderscoreRule); } }

    private static bool CanHaveTheMRemoved(FieldDeclarationSyntax fieldDeclaration, SemanticModel semanticModel)
    {
        if (!fieldDeclaration.Modifiers.Any(SyntaxKind.PrivateKeyword))
        {
            return false;
        }

        var token = fieldDeclaration.Declaration.GetLastToken();
        return token.Text.StartsWith("m_");
    }

    public void AnalyzeNode(SyntaxNode node, SemanticModel semanticModel, Action<Diagnostic> addDiagnostic, CancellationToken cancellationToken)
    {
        if (CanHaveTheMRemoved((FieldDeclarationSyntax)node, semanticModel))
        {
            addDiagnostic(Diagnostic.Create(RemoveMUnderscoreRule, node.GetLocation()));
        }
    }
}

/// <summary>
/// this is used to integrate within Visual Studio refactor capabilities
/// </summary>
[ExportCodeFixProvider("NoMUnderscore", LanguageNames.CSharp)]
internal class CodeFixProvider : ICodeFixProvider
{
    public IEnumerable<string> GetFixableDiagnosticIds()
    {
        return new[] { FieldsDoNotStartWithMUnderscore.RemoveMDiagnosticId };
    }

    public async Task<IEnumerable<CodeAction>> GetFixesAsync(Document document, TextSpan span, IEnumerable<Diagnostic> diagnostics, CancellationToken cancellationToken)
    {
        var root = await document.GetSyntaxRootAsync(cancellationToken);
        var diagnosticSpan = diagnostics.First().Location.SourceSpan;
        var declaration = root.FindToken(diagnosticSpan.Start).Parent.AncestorsAndSelf().OfType<FieldDeclarationSyntax>().First();
        return new[] { CodeAction.Create(FieldsDoNotStartWithMUnderscore.RemoveMUnderscoreRule.Description, c => RemoveMAsync(document, declaration, c)) };
    }

    private async Task<Document> RemoveMAsync(Document document, FieldDeclarationSyntax fieldDeclaration, CancellationToken cancellationToken)
    {
        var nameToken = fieldDeclaration.Declaration.GetLastToken();
        var newNameToken = SyntaxFactory.Identifier(nameToken.Text.Replace("m_", ""));

        var variableDeclarationSyntax = fieldDeclaration.Declaration.ReplaceToken(nameToken, newNameToken);

        var newLocal = fieldDeclaration.WithDeclaration(variableDeclarationSyntax);

        var formattedLocal = newLocal.WithAdditionalAnnotations(Formatter.Annotation);

        var originalRoot = await document.GetSyntaxRootAsync(cancellationToken);
        var newSyntaxRoot = originalRoot.ReplaceNode(fieldDeclaration, formattedLocal);

        return document.WithSyntaxRoot(newSyntaxRoot);
    }
}
```

This require quite a bit of code. However, it is possible to regroup and refactor common operation on certain elements (fields, constructors, etc.) to reduce the amount of code.

> Of course, this code is not production ready and not unit tested. Do not take it as is. It is full of bug and is not ready for any type of environment. This is only to show what is possible.

### Conclusion

This of course is just the beginning. I've just showed you what is possible. It took me less than an hour to prepare those two examples. With more time, it could be possible to create some very complex scenarios of very high quality.

We're living in a crazy world right now. We're getting more and more control over the code that we write. The possibilities that are opening up when we can interact to something as low-level as the compiler are just breathtaking.

For me, it's tools that are built for the developers, for our needs and of course&hellip; fun to use.

### Links

[Get Roslyn Now](http://msdn.microsoft.com/en-US/roslyn)

[Roslyn Roadmap](http://roslyn.codeplex.com/wikipage?title=Roadmap&amp;referringTitle=Home "Roadmap&amp;referringTitle=Home")

[Language Features implementation status](https://roslyn.codeplex.com/wikipage?title=Language%20Feature%20Status&amp;referringTitle=Documentation "Language%20Feature%20Status&amp;referringTitle=Documentation")

[Roslyn Sample and Walkthrough](https://roslyn.codeplex.com/wikipage?title=Samples%20and%20Walkthroughs&amp;referringTitle=Home "Samples%20and%20Walkthroughs&amp;referringTitle=Home")
