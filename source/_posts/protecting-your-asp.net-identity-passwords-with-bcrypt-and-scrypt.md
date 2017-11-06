---
title: "Protecting your ASP.NET Identity passwords with bcrypt and scrypt"
date: 2016-07-18 08:00:00
tags: [asp.net core, security]
---

Most people nowadays use sort of authentication mechanism when coding a new website. Some people are connected directly with Active Directory, others use Social login like Google, Facebook or Twitter.

Then there is all those enterprise/edge case customers that don't have an SSO (or can't have one) and still require users to create an account and pick a password.

In those scenarios, you don't want to end up on [Troy Hunt](https://www.troyhunt.com/)'s infamous list of [Have I been pwned?](https://haveibeenpwned.com/). If you do, you want the maximum amount of time so that you can change your password everywhere. We still do too much [password reuse](https://www.troyhunt.com/science-of-password-selection/). It's bad but it's not going away anytime soon.

So how do you delay? First, [do not store your password in clear text](https://cwe.mitre.org/data/definitions/311.html). Then, hash/salt them. But which hashing algorithm should you use?

Most of .NET (pre-core) suggested MD5/SHA1 as a default hashing mechanism which is [highly unsafe](). In .NET Core, the default implementation is [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) which is a hundred times better. However, unless you require FIPS certification, is not exactly safe either.

### Slower algorithms

PBKDF2 is part of a family of algorithms that allow you to configure work factors at the moment of encoding the password. PBKDF2 allow you to set the amount of iterations that the hash must run before the hash is returned.

But given enough CPU/memory, this can be cracked faster each year.

There come bcrypt and scrypt.

BCrypt, like PBKDF2, allow you to set a work factor that will make the CPU run more heavily to generate a single hash. This makes brute forcing algorithms slower to run. However, with [GPU hashing](), those limitations are less and less of a restriction.

SCrypt on the other hand, allows you to set the memory usage. Making generating a lot of password a very memory and CPU intensive process. This makes GPU hashing way harder for that specific algorithm.

### Which one do I choose?

If you need FIPS certification? PBKDF2. Otherwise, scrypt is the way to go.

PBKDF2 should be configured to at least 10,000 iterations. Scrypt should be configured so that a server is *responsive* enough for users to log in. That means less than a second to login. Many parameters are offered and will need to be tweaked to your current hardware.

Please not that I am not a mathematician and I can't explain to you why one is better than the other. I'm just relaying what was suggested on [OWASP - Password Storage Cheat Sheet](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet#Impose_infeasible_verification_on_attacker).

### How do I install those algorithm within ASP.NET Identity?

I have provided two different sample implementation for bcrypt and scrypt that will replace the default ASP.NET Core Identity `IPasswordHasher`.

I have also included the ASP.NET Core compatible package to use when you want to install this package.


#### BCrypt

```powershell
Install-Package BCrypt.Net-Core
```

This package was created by [Stephen Donaghy](https://www.stephendonaghy.com/?p=38) as a direct port of bcrypt. [Source on GitHub](https://github.com/neoKushan/BCrypt.Net-Core).

Startup.cs:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddTransient<IPasswordHasher<ApplicationUser>, BCryptPasswordHasher>();
}
```

```csharp
public class BCryptPasswordHasher : IPasswordHasher<ApplicationUser>
{
    public string HashPassword(ApplicationUser user, string password)
    {
        return BCrypt.Net.BCrypt.HashPassword(SaltPassword(user, password), 10);
    }

    public PasswordVerificationResult VerifyHashedPassword(ApplicationUser user, string hashedPassword, string providedPassword)
    {
        if (BCrypt.Net.BCrypt.Verify(SaltPassword(user, providedPassword), hashedPassword))
            return PasswordVerificationResult.Success;

        return PasswordVerificationResult.Failed;
    }

    private string SaltPassword(ApplicationUser user, string password)
    {
        //TODO: salt password
    }
}
```

#### SCrypt

```powershell
Install-Package Scrypt.NETCore
```
Startup.cs:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddTransient<IPasswordHasher<ApplicationUser>, SCryptPasswordHasher>();
}
```

```csharp
public class SCryptPasswordHasher : IPasswordHasher<ApplicationUser>
{
    private readonly ScryptEncoder _encoder;

    public SCryptPasswordHasher()
    {
        _encoder = new ScryptEncoder(2 ^ 16, 8, 1);
    }

    public string HashPassword(ApplicationUser user, string password)
    {
        return _encoder.Encode(SaltPassword(user, password));
    }

    public PasswordVerificationResult VerifyHashedPassword(ApplicationUser user, string hashedPassword, string providedPassword)
    {
        if (_encoder.Compare(SaltPassword(user, providedPassword), hashedPassword))
            return PasswordVerificationResult.Success;

        return PasswordVerificationResult.Failed;
    }

    private string SaltPassword(ApplicationUser user, string password)
    {
        //TODO: salt password
    }
}
```

### More reading

Read more on :

* [scrypt](https://en.wikipedia.org/wiki/Scrypt)
* [bcrypt](https://en.wikipedia.org/wiki/Bcrypt)
* [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2)
