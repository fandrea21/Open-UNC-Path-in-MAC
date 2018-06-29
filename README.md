# Open-UNC-Path-in-MAC

on searchReplace(theText, SearchString, ReplaceString)
	set OldDelims to AppleScript's text item delimiters
	set AppleScript's text item delimiters to SearchString
	set newText to text items of theText
	set AppleScript's text item delimiters to ReplaceString
	set newText to newText as text
	set AppleScript's text item delimiters to OldDelims
	return newText
end searchReplace

on run {input, parameters}
	
	set myClip to the input
	set mylocation to searchReplace(myClip, "<", "")
	set mylocation to searchReplace(mylocation, ">.", "")
	set mylocation to searchReplace(mylocation, ">", "")
	set mylocation to searchReplace(mylocation, "\\", "/")
	set mylocation to "smb:" & mylocation
	# convert Windows network drive paths to SMB addresses EXAMPLE:
	set mylocation to searchReplace(mylocation, "smb:Z:", "smb://192.168.0.2/STORAGE/")
	# check if the person who gave you the windows link used a lowercase drive letter:
	set mylocation to searchReplace(mylocation, "smb:z:", "smb://192.168.0.2/STORAGE/")
	# fix issue with spaces
	set mylocation to searchReplace(mylocation, " ", "%20")
	
	
	tell application "Finder"
		open location mylocation
	end tell
	
	# after setting the location, set Finder to topmost, or delete this section if you dont want that.
	tell application "Finder"
		activate
	end tell
	
	
	return input
end run
