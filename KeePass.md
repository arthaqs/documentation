1. Make it impossible to brute-force your Master password:
	1. File -> Database Setting
	2. Security tab
	3. Click 1 Second Delay

Although this increases time when you are unlocking your KeePass database to 1 second (which is actually nothing, try it!), it also makes any brute-force attacks impossible to break your password.
The password is hashed the number of times the number in `iterations` field appeared when you clicked `1 Second Delay` button. For me, its `78 614 528` times.
Example: If your password is 15 characters, contains number, special character, uppercase/lowercase letters, it will take $10^{22}$ years years to crack it - which is 10 sextillion years - or about 724 trillion times the current age of the universe. It's most probably not crackable by current quantum computers.


2. Auto-fill forms in web:
   ![[keepass-auto-type.png|212x95]]

