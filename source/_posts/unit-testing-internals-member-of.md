---
title: "Unit testing Internals member of a solution from another project"
date: 2009-02-13 23:44:00
tags: [unit test]
---

Here is a little bit of knowledge that lots of people are not aware of. There is an Attribute that is [InternalsVisibleToAttribute](http://msdn.microsoft.com/en-us/library/system.runtime.compilerservices.internalsvisibletoattribute.aspx "InternalsVisibleToAttribute") that allows access to a specific external project (the unit test project).

This attribute, once inserted inside AssemblyInfo.cs, will grant "public" visibility to all internal members of the project to the project specified within the attribute.

Here is how it is shown on MSDN:

```cs
[assembly:InternalsVisibleTo("MyAssembly, PublicKey=32ab4ba45e0a69a1")]
```

It is however wrong and will never work. The main reason is that what is there let us believe that it's the **PublicKeyToken** but it is in fact the **PublicKey** as clearly typed there.

So... how do we get this PublicKey? By executing the following command: `sn -Tp MyAssembly.dll`

The result is going to be something like the following:

```
 Public key is
         0024000004800000940000000602000000240000525341310004000001000100adfedd2329a0f8
         3e057f0b14e47f02ec865e542c2dcca6349177fe3530edd5080276c48c6d02fa0a6f67738cc1a0
         793be3322cf17b8995acc15055c00fa61b67a203c7eb2516922810ff0b17cd2e08492bdcafc4a9
         23e6fff4caba672a4c2d0d0f5cac9aea95c3dce3717bb733d852c387f5f025c42c14ec8d759f7e
         b13689be
 Public key token is 96dfc321948ee54c
```

Here is the end result to make it properly visible:

```cs
[assembly: InternalsVisibleTo("AssemblyB, PublicKey="
        + "0024000004800000940000000602000000240000525341310004000001000100adfedd2329a0f8"
        + "3e057f0b14e47f02ec865e542c2dcca6349177fe3530edd5080276c48c6d02fa0a6f67738cc1a0"
        + "793be3322cf17b8995acc15055c00fa61b67a203c7eb2516922810ff0b17cd2e08492bdcafc4a9"
        + "23e6fff4caba672a4c2d0d0f5cac9aea95c3dce3717bb733d852c387f5f025c42c14ec8d759f7e"
        + "b13689be" )]
```

After this step is done, all reference to internal members are considered "public" for this specific project. This simple trick allows you to complete your tests and don't gives any excuse not to test.
