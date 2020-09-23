<div align="center">

## Complete working Basic Authentication


</div>

### Description

This code shows your visitors the Basic Authentication dialog (or NT Login Dialog)

It also returns the password and the username

If you like it, please vote for this 16 year old programmer :o)
 
### More Info
 
In the dialog the username and password (/and domain)

Paste it and run it. It does not verify any usernames or so.

The password and username given by the visitors of your site

Protects your site :o))


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Almar Joling](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/almar-joling.md)
**Level**          |Beginner
**User Rating**    |4.6 (128 globes from 28 users)
**Compatibility**  |ASP \(Active Server Pages\)
**Category**       |[Security](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/security__4-14.md)
**World**          |[ASP / VbScript](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/asp-vbscript.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/almar-joling-complete-working-basic-authentication__4-6300/archive/master.zip)

### API Declarations

The Base64 decryption algorithm I used came from http://www.aspcode.net. Showing the dialog is completely by me


### Source Code

```
<%
Response.Buffer = True
Response.Clear
Dim Myname, MyPass
GetUser Myname, MyPass
Response.Write MyName & "->" & MyPass
if len(Myname) = 0 then
 Response.Status = "401 Unauthorized"
 Response.AddHeader "WWW-Authenticate","BASIC Realm=enter your realm here"
 Response.End
End If
Sub GetUser(LOGON_USER, LOGON_PASSWORD)
 Dim UP, Pos, Auth
 Auth = Request.ServerVariables("HTTP_AUTHORIZATION")
 LOGON_USER = ""
 LOGON_PASSWORD = ""
 If LCase(Left(Auth, 5)) = "basic" Then
  UP = Base64Decode(Mid(Auth, 7))
  Pos = InStr(UP, ":")
  If Pos > 1 Then
   LOGON_USER = Left(UP, Pos - 1)
   LOGON_PASSWORD = Mid(UP, Pos + 1)
  End If
 End If
End Sub
' Decodes a base-64 encoded string.
Function Base64Decode(base64String)
 Const Base64CodeBase = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
 Dim dataLength, Out, groupBegin
 dataLength = Len(base64String)
 Out = ""
 If dataLength Mod 4 <> 0 Then
  Err.Raise 1, "Base64Decode", "Bad Base64 string."
  Exit Function
 End If
 ' Now decode each group:
 For groupBegin = 1 To dataLength Step 4
  Dim numDataBytes, CharCounter, thisChar, thisData, groupData
  ' Each data group encodes up To 3 actual bytes.
  numDataBytes = 3
  groupData = 0
  For CharCounter = 0 To 3
   ' <B>Convert</B> each character into 6 bits of data, And add it To
   ' an integer For temporary storage. If a character is a '=', there
   ' is one fewer data byte. (There can only be a maximum of 2 '=' In
   ' the whole string.)
   thisChar = Mid(base64String, groupBegin + CharCounter, 1)
   If thisChar = "=" Then
    numDataBytes = numDataBytes - 1
    thisData = 0
   Else
    thisData = InStr(Base64CodeBase, thisChar) - 1
   End If
   If thisData=-1 Then
    Err.Raise 2, "Base64Decode", "Bad character In Base64 string."
    Exit Function
   End If
   groupData = 64 * groupData + thisData
  Next
  ' Convert 3-byte integer into up To 3 characters
  Dim OneChar
  For CharCounter = 1 To numDataBytes
   Select Case CharCounter
    Case 1: OneChar = groupData \ 65536
    Case 2: OneChar = (groupData And 65535) \ 256
    Case 3: OneChar = (groupData And 255)
   End Select
   Out = Out & Chr(OneChar)
  Next
 Next
 Base64Decode = Out
End Function
%>
```

