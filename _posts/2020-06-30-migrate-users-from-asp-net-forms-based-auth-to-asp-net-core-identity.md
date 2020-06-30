---
layout: post
published: false
title: Migrate users from ASP.NET Forms-based Auth to ASP.NET Core Identity
---
Do you have a very old ASP forms based app? Do you want to run `dotnet new webapp` and migrate your pages, but you don't know how you'll simply move the users over to the new world? Well, let me give you a solution *if* you are moving over to ASP.Net Core Identity. If you don't know what that is, think about it like new forms auth, I suppose.

We're going from [this](https://support.microsoft.com/en-us/help/308157/how-to-implement-forms-based-authentication-in-your-asp-net-applicatio) to [this](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-3.1&tabs=netcore-cli).

At least that's what I'm going to cover. Basically the idea of having a brand new site, but I add all of my existing users to the backend. No re-registration, no changes for the users if you can change the app behavior to match. Just easy.

You can check the second link up above to see what the end table looks like, but for now I'm going to just talk about the rest of this situation like you have an old app and plain text username/password pairs.

## Let's begin

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

This program can now be [compiled into a single file](https://dotnetcoretutorials.com/2019/06/20/publishing-a-single-exe-file-in-net-core-3-0/) (if you want) using `dotnet publish -r win-x64 -c Release /p:PublishSingleFile=true` if you want. That's what I did. At a minimum, i'd recommend building it in release mode as that makes it run much faster.

I went ahead and put this code [here](https://github.com/royashbrook/CLI-PasswordHasher), but you can just follow the steps above to make it yourself.

## ok, so what now?

OK, well at this point, I have to assume a few things. I have to assume that:
- You already have a `dotnet new webapp` with the identity added
- You have a database setup to support that
- You know a little bit about powershell and editing files
- You know a little bit about SQL, at least to run some commands

I'm not going to go over setting up the webapp, but I already linked to the setup up top.

So!!!! Since we have a database setup that we want to inject our users into, we just need a list of users and passwords and a way to generate our sql commands. I'm just going to use powershell for this. So we will have a txt file called users.txt that looks something like this. I'm using a tab seperated file just to make things easy to ready, but you could use a regular csv or whatever.

```txt
Username    Password
User1   Password1
User2   Password2
User2   Password3
```

Now assuming  you are in the same folder as users.txt, and in powershell, you can run this command

`$users = ipcsv users.txt -Delimiter "\t"`

Now that you have the user data in memory in $users, you can swap to your directory with the hasing executable and run something like this:

`$hashes = $logins | select Username, @{n="Hash";e={.\h.exe $_.Password}}`

This assumes that you kept the property names above, and that you are using h.exe. You can change things accordingly. This will now give you a list of usernames and new password hashes. Now let's just gen a little sql code. I am basically going to use a CTE (if you don't know what that is, let's say it's like an inline temp table in sql) to process this list of users and feed it to an insert statement with some other values. So first we'll generate a big union queries for all of our users like this:

`$hashes | %{"union all select '{0}','{1}'" -f $_.UserName,$_.Hash}`

This will generate something like this:

```txt
union all select 'User1','somelonghashvalue'
union all select 'User2','somelonghashvalue'
union all select 'User3','somelonghashvalue'
```

Now we are going to go to our database and insert these values like this:

```SQL
;with a(UserName,Hash) as (
--union all
select 'User1','somelonghashvalue'
union all select 'User2','somelonghashvalue'
union all select 'User3','somelonghashvalue'
)
insert into [dbo].[AspNetUsers] ([Id],[SecurityStamp],UserName,[NormalizedUserName],[EmailConfirmed],[PasswordHash],[TwoFactorEnabled],[LockoutEnabled],AccessFailedCount,PhoneNumberConfirmed)
select newid(),newid(),UserName, UPPER(UserName),1,[Hash],0,0,0,0 from a
```

Note that I am removing/commenting out the *first* union all command and changing it to simply a select. Hopefully if you are doing this, you know why and this makes sense.

## Profit...

That's it. Your users should be able to login. Or you should be able to login as them if you want to test it. You need the extra 1 and 0 values and the newid() lines or else they can't. I normalized to upper since that what it appears the identity logic does itself, but i guess that could be anything. I just tried to keep it as simple as possible.


## Final Thoughts

This is *really* only meant to be a very temporary solution to get over a hump. An old app that you can make work the same, but you don't have a great way to coordinate a release with the users. You can a/b test it, you can validate it all fully, and you have no reason to think anyone will notice or that the communication will cause more issues than it is worth. To me closing the security holes and other issues with a 15 year old code base is worth it, but this will be followed up with an initiative to migrate to a PWA and get into a real CI/CD cycle so this can be maintained easier moving forward. I don't recommend doing this and then sticking with this.

But if you do **need** to stick with this a bit, you can [scaffold](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/scaffold-identity?view=aspnetcore-3.1&tabs=netcore-cli) the identity pages, and customize items or follow [this article](https://andyp.dev/posts/disable-user-registrations-in-asp-net-core-3-identity) to disable user registration. As mentioned there, you'll need a way to register apps, but if you are stuck in between this place and the next place, at least you can generate the hashes using this method and simply insert new users or build your own internal registration app.

## References

There were many references I looked at while I was working on this. Here are some of the ones I found most useful:
- https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-3.1&tabs=netcore-cli
- https://docs.microsoft.com/en-us/aspnet/core/security/authentication/scaffold-identity?view=aspnetcore-3.1&tabs=netcore-cli
- https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs
- https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasherOptions.cs
- https://andrewlock.net/exploring-the-asp-net-core-identity-passwordhasher/
- https://andrewlock.net/migrating-passwords-in-asp-net-core-identity-with-a-custom-passwordhasher/
- https://kenhaggerty.com/articles/article/aspnet-core-31-password-hasher
- https://stackoverflow.com/questions/20621950/asp-net-identitys-default-password-hasher-how-does-it-work-and-is-it-secure
- https://docs.microsoft.com/en-us/aspnet/core/security/authorization/simple?view=aspnetcore-3.1
- https://andyp.dev/posts/disable-user-registrations-in-asp-net-core-3-identity
- https://docs.microsoft.com/en-us/aspnet/core/security/authentication/identity-configuration?view=aspnetcore-3.1#password
- https://code-maze.com/identity-asp-net-core-project/
- https://docs.microsoft.com/en-us/aspnet/web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
- https://support.microsoft.com/en-us/help/308157/how-to-implement-forms-based-authentication-in-your-asp-net-applicatio
- https://dotnetcoretutorials.com/2019/06/20/publishing-a-single-exe-file-in-net-core-3-0/
- https://community.spiceworks.com/how_to/153476-how-to-discover-types-classes-in-powershell





