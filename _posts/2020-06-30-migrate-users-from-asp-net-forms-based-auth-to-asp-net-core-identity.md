---
layout: post
published: false
title: Migrate users from ASP.NET Forms-based Auth to ASP.NET Core Identity
---
Do you have a very old ASP forms based app? Do you want to run `dotnet new webapp` and migrate your pages, but you don't know how you'll simply move the users over to the new world? Well, let me give you a solution *if* you are moving over to ASP.Net Core Identity. If you don't know what that is, think about it like new forms auth, I suppose.

We're going from [this](https://support.microsoft.com/en-us/help/308157/how-to-implement-forms-based-authentication-in-your-asp-net-applicatio) to [this](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-3.1&tabs=netcore-cli).

At least that's what I'm going to cover. Basically the idea of having a brand new site, but I add all of my existing users to the backend. No re-registration, no changes for the users if you can change the app behavior to match. Just easy.

You can check the second link up above to see what the end table looks like, but for now I'm going to just talk about the rest of this situation like you have an old app and plain text username/password pairs.

Short version of this story is I wrote an app to generate the hashes I needed (based on the source from dotnetcore on github) and then just injected guids into the other fields that needed information. Probably not perfect, but it seems to work fine.

Here are the sources for the relevant code from dotnet core identity:
- https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs
- https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasherOptions.cs

I am going to tweak these a bit for our purposes. First of all, I want to skip the options, secondly I don't want to have to pass in a user, I just want to put in a password and get the pass.

So let's make an app that does that. I'll creatively call it 'h' for hasher. You can call it whatever you want though, just change 'h' below to something else. =)

``` PowerShell
# make the base project
dotnet new console -o h

# switch to project dir
cd h

# add a couple of packages we'll need
dotnet add package Microsoft.AspNetCore.Identity
dotnet add package Microsoft.AspNetCore.Cryptography.KeyDerivation

# get the source file we'll tweak from github
$uri = "https://raw.githubusercontent.com/aspnet/Identity/master/src/Core/"
"PasswordHasher.cs" | %{iwr $uri$_ -o .\$_}

```

Now we need to make some tweaks to the files. First of all, we'll just copy, paste the following code into the program.cs. Again, replace the namespace with h if you named it something else. You can also tweak this if you want to some other structure, but this is what I went with.

```Csharp
using System;

namespace h
{
    class Program
    {
        static void Main(string[] args)
        {
            string result = "Syntax: hash <password> or hash <password> <hash>";
            if (args.Length==1 || args.Length==2){
                var passwordHasher = new Microsoft.AspNetCore.Identity.PasswordHasher();
                if(args.Length == 1)
                    result = passwordHasher.HashPassword(args[0]);
                if(args.Length == 2)
                    result = (passwordHasher.VerifyHashedPassword(args[1],args[0]).ToString() == "Success").ToString();
            }
            Console.WriteLine(result);
        }
    }
}

```

Now we need to update the PasswordHasher.cs file to remove the need for a user to be pushed in. This only requires a few minor changes:

```Csharp

//change this:
public class PasswordHasher<TUser> : IPasswordHasher<TUser> where TUser : class

//to this, or just remove everything after the comment below
public class PasswordHasher//<TUser> : IPasswordHasher<TUser> where TUser : class

//next, simply add some default values for these private vars

//these...
private readonly PasswordHasherCompatibilityMode _compatibilityMode;
private readonly int _iterCount;
private readonly RandomNumberGenerator _rng;

//become these...
//Note these are the default values from the PasswordHasherOptions.cs file
// you can see that here if you want:
//https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasherOptions.cs
private readonly PasswordHasherCompatibilityMode _compatibilityMode = PasswordHasherCompatibilityMode.IdentityV3;
private readonly int _iterCount = 10000;
private readonly RandomNumberGenerator _rng = RandomNumberGenerator.Create();

//next we are going to, basically, wipe out the constructor
// as we don't need to pass in any information

//replace this...
public PasswordHasher(IOptions<PasswordHasherOptions> optionsAccessor = null)
{
  var options = optionsAccessor?.Value ?? new PasswordHasherOptions();

  _compatibilityMode = options.CompatibilityMode;
  switch (_compatibilityMode)
  {
    case PasswordHasherCompatibilityMode.IdentityV2:
      // nothing else to do
      break;

    case PasswordHasherCompatibilityMode.IdentityV3:
      _iterCount = options.IterationCount;
      if (_iterCount < 1)
      {
        throw new InvalidOperationException(Resources.InvalidPasswordHasherIterationCount);
      }
      break;

    default:
      throw new InvalidOperationException(Resources.InvalidPasswordHasherCompatibilityMode);
  }

  _rng = options.Rng;
}

// with just this one line:
public PasswordHasher(){}

// and finally, we need to remove the user var from these two function calls
// so these lines...
public virtual string HashPassword(TUser user, string password)
public virtual PasswordVerificationResult VerifyHashedPassword(TUser user, string hashedPassword, string providedPassword)
  
//become these lines...
public virtual string HashPassword(string password)
public virtual PasswordVerificationResult VerifyHashedPassword(string hashedPassword, string providedPassword)
```

That's it. Basically we are just avoiding the use of the options class and removing dependency on the user object. I believe in my reading I found that there was a convention/reason to keep that in the interface, but it's not actually used inside this class at all, so simple to remove so we can use the same hashing mechanism.

Now we can build and run it. Here's some powershell CLI output for this:
```PowerShell
[C:\git\h]> dotnet build
Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.
  h -> C:\git\h\bin\Debug\netcoreapp3.1\h.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.51
└[C:\git\h]> $passwordHash = dotnet run password
└[C:\git\h]> $password1Hash = dotnet run password1
└[C:\git\h]> $passwordHash
AQAAAAEAACcQAAAAEKBC6Tb5klZXeE51B8GwBssJWHVyqAy4O3BWPEVkaNzYx6XttTBXNuUYlI6Bu4sGFQ==
└[C:\git\h]> $password1Hash
AQAAAAEAACcQAAAAEEO6oT/nnLhprMbx2TwFhaM3WLdylwAS5bNVwAeWI9t0jD1BBLgL0+gcYmFS5y+7og==
└[C:\git\h]> dotnet run password $passwordHash
True
└[C:\git\h]> dotnet run password $password1Hash
False
└[C:\git\h]> dotnet run password1 $password1Hash
True
└[C:\git\h]> dotnet run password1 $passwordHash
False
```

This program can now be compiled into a single file (if you want) using this:

