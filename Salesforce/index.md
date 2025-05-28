---
publish on andju-know: true
---
# Salesforce
Helpful tips and tricks
## Convert Salesforce IDs
Each record in Salesforce can be identified with an unique id - of which two versions exist: One (case-sensitive) with 15-characters and one (case-insensitive) with 18-characters.
### Excel
This [page](https://www.gammone.com/en/programming/how-to-convert-salesforce-id-from-15-to-18-chars) describes the algorithm to convert a 15-character ID into its 18-character equivalent. It also provides code implementations for different languages - a.o. Visual Basic for Applications (VBA):
```vb.net
Function FixID(InID As String) As String
	If Len(InID) = 18 Then
		FixID = InID
		Exit Function
	End If
	Dim InChars As String, InI As Integer, InUpper As String
	Dim InCnt As Integer
	InChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ012345"
	InUpper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"

	InCnt = 0
	For InI = 15 To 1 Step -1
		InCnt = 2 * InCnt + Sgn(InStr(1, InUpper, Mid(InID, InI, 1), vbBinaryCompare))
		If InI Mod 5 = 1 Then
			FixID = Mid(InChars, InCnt + 1, 1) + FixID
			InCnt = 0
		End If
	Next InI
	FixID = InID + FixID
End Function
```

This code allows you to get an Excel-function (in your current workbook) to do the conversion.

> [!warning] Excel formula
> The page also provides an Excel formula - but at least for me it did not work.

> [!info] Add the VBA code to Excel
> 1. Press Alt+F11 to open the VBA Editor
> 2. Right click on VBAProject (left side of the screen) ➡️ Insert ➡️ Module
> 3. Copy & Paste the VBA code
> 4. Now you have a new Excel function (`=FixID()`) available to perform the conversion
### Browser
The bookmarklet described on this [page](https://help.salesforce.com/s/articleView?id=000385066&type=1) allows you to convert a single ID:
```js
javascript:(function(){
var input=prompt('Enter 15-character ID');
var output;
	if(input.length == 15){
		var addon="";
		for(var block=0;block<3; block++)
		{
			var loop=0;
			for(var position=0;position<5;position++){
				var current=input.charAt(block*5+position);
				if(current>="A" && current<="Z")
					loop+=1<<position;
			}
			addon+="ABCDEFGHIJKLMNOPQRSTUVWXYZ012345".charAt(loop);
		}
		output=(input+addon);
	}
	else{
		alert("Error : "+input+" isn't 15 characters ("+input.length+")");
		return;
	}
prompt('18-character ID:',output);
})();
```

Just add a new bookmark to your browser and enter the code as the URL.

## Reset security token by URL
If the option to reset your security token is not available in your settings (e.g. in a Sandbox), you can use the following URL:
```http
https://<YourDomain>.lightning.force.com/_ui/system/security/ResetApiTokenEdit?retURL=%2Fui%2Fsetup%2FSetup%3Fsetupid%3DPersonalInfo&setupid=ResetApiToken
```