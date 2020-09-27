# Skid

In the storage we can find a session:

> eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiIsImtpZCI6InNraWQifQ.eyJ1c2VybmFtZSI6InNraWQifQ.sacXoUrQCXpaylE4a4RGrCawHqBJJVGfOozOaPxQqOo

We can decode some parts of this using a JWT token decoder:

```json
{
  "typ" : "JWT",
  "alg" : "HS256",
  "kid" : "skid"
}
```

```json
{
  "username" : "skid"
}
```

When you change the username to something else like Congon4tor the site will throw an Internal Server Error until you delete the session.
There has to be some way to sign the token but I can't figure it out.
