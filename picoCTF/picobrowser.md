# Overview
<img width="1919" height="595" alt="Screenshot_2025-09-30_14-40-25" src="https://github.com/user-attachments/assets/db0aa533-c534-486a-ac08-645e93de3812" />
The website is just that simple
Clicked on the flag give me a warning that my browser does not have access to get the flag. Should I download the right browser? Nope, I just need to observe and get the request.

Open developer tools, access `network ` tab then click on the `get the flag` button 
<img width="1920" height="934" alt="Screenshot_2025-09-30_16-06-29" src="https://github.com/user-attachments/assets/b744f580-cae1-44d1-bf9a-bd74ad15b597" />

Then, right click and copy the link as cURL. I'll just put the payload right here.
```
curl 'https://jupiter.challenges.picoctf.org/problem/28921/flag' --compressed -H 'User-Agent: picobrowser' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' -H 'Accept-Encoding: gzip, deflate, br, zstd' -H 'Referer: https://jupiter.challenges.picoctf.org/problem/28921/flag' -H 'Connection: keep-alive' -H 'Cookie: cf_clearance=8bFi01CWbENIMNKFMVGKlsYtSwS_hzLBTDq499YDJX4-1759148619-1.2.1.1-zNnTDGztkzFFMSRjRVa35sK2UcBxjVl6dgtW6UOwN198hs7Dg9E6h7K2uFfq8OfCMCX0_6wt0gfaoTSGunZd5QxqDD1FuOmPM_xySMHXZRUWLc8XXLtuvqEEr4O2ncRCz4.yEGrqvO2aCuIHD.LkD03B6KVoU80VeIzr6FItuWPPezCta15cplqMPdL9vi7q6wiUyW85wCRe0z.7mKXzsR.J_Io3i5pkIqUP2eXB5Rg; _ga=GA1.2.540776041.1757241273; _ga_L6FT52K063=GS2.2.s1759206740$o36$g1$t1759206885$j52$l0$h95617507; _ga_BSZFGM3NWK=GS2.1.s1759206721$o3$g1$t1759206737$j44$l0$h897516762; _gid=GA1.2.1662669962.1758782623' -H 'Upgrade-Insecure-Requests: 1' -H 'Sec-Fetch-Dest: document' -H 'Sec-Fetch-Mode: navigate' -H 'Sec-Fetch-Site: same-origin' -H 'Sec-Fetch-User: ?1' -H 'Priority: u=0, i'
```
For the `User-agent` I manually modified it into `picobroser` . Then I got the flag.

```
<!DOCTYPE html>
<html lang="en">

<head>
    <title>My New Website</title>


    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">

    <link href="https://getbootstrap.com/docs/3.3/examples/jumbotron-narrow/jumbotron-narrow.css" rel="stylesheet">

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>

</head>

<body>

    <div class="container">
        <div class="header">
            <nav>
                <ul class="nav nav-pills pull-right">
                    <li role="presentation" class="active"><a href="/">Home</a>
                    </li>
                    <li role="presentation"><a href="/unimplemented">Sign In</a>
                    </li>
                    <li role="presentation"><a href="/unimplemented">Sign Out</a>
                    </li>
                </ul>
            </nav>
            <h3 class="text-muted">My New Website</h3>
        </div>
        
       <!-- Categories: success (green), info (blue), warning (yellow), danger (red) -->
       
       
       <div class="alert alert-success alert-dismissible" role="alert" id="myAlert">
         <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
         <!-- <strong>Title</strong> --> picobrowser!
           </div>
     
     
     
        <div class="jumbotron">
            <p class="lead"></p>
            <p style="text-align:center; font-size:30px;"><b>Flag</b>: <code>picoCTF{p1c0_s3cr3t_ag3nt_84f9c865}</code></p>
            <!-- <p><a class="btn btn-lg btn-success" href="admin" role="button">Click here for the flag!</a> -->
            <!-- </p> -->
        </div>


        <footer class="footer">
            <p>&copy; PicoCTF 2019</p>
        </footer>

    </div>
    <script>
    $(document).ready(function(){
        $(".close").click(function(){
            $("myAlert").alert("close");
        });
    });
    </script>
</body>

</html>
```

Happy Hacking!!!!
