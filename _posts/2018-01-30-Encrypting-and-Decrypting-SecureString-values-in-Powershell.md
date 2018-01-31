---
layout: post
title: Encrypting and Decrypting SecureString values in Powershell
---

Here are some ways to encrypt and decrypt a securestring using powershell

``` powershell
#plaintext password
$pt= "P@ssword1"
$pt

#convert to secure string object
$oss= $pt | ConvertTo-SecureString -AsPlainText -Force
$oss

#secure string object as a string, this is what you store in a file
$sss= $oss| ConvertFrom-SecureString
$sss

#now how to decrypt?

#Get the string value back into an object?
$noss = $sss | ConvertTo-SecureString
$noss

#can either convert the object using these calls
$doss = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($noss)
$doss = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($doss)
$doss

#or we can use a credential to do it for us like this
(New-Object System.Net.NetworkCredential "",$noss).Password
#or like this (have to give PSCredential a username value, not blank)
(New-Object PSCredential "u",$noss).GetNetworkCredential().Password

#of course if you are are using something that needs a credential
#you can usually simply pass it the securestring
#but if, for some reason, you want to obfuscate some data this way, you can
```