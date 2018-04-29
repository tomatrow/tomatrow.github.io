# Notes on node-oidc-provider tutorial

## Steps 

### 00 

Running through this on Heroku is easy enough. I ran into some login issues. It might just be I authenticated with them a long time ago, and it seems the've been updated their tools in the background. 

Thanks to the wonderful error feedback and a lettle bit of background knowledege, I was able to piece together this url `https://young-reef-89427.herokuapp.com/auth?client_id=foo&response_type=code&scope=openid` that *actually* did something.

Following step 7 produces: 

```
{"authorization_endpoint":"https://young-reef-89427.herokuapp.com/auth","claims_parameter_supported":false,"claims_supported":["auth_time","iss","sub"],"grant_types_supported":["implicit","authorization_code","refresh_token"],"id_token_signing_alg_values_supported":["none","HS256","HS384","HS512","RS256"],"issuer":"https://young-reef-89427.herokuapp.com","jwks_uri":"https://young-reef-89427.herokuapp.com/certs","request_object_signing_alg_values_supported":["none","HS256","HS384","HS512","RS256","RS384","RS512","PS256","PS384","PS512","ES256","ES384","ES512"],"request_parameter_supported":false,"request_uri_parameter_supported":true,"require_request_uri_registration":true,"response_modes_supported":["form_post","fragment","query"],"response_types_supported":["code id_token token","code id_token","code token","code","id_token token","id_token","none"],"scopes_supported":["openid","offline_access"],"subject_types_supported":["public"],"token_endpoint":"https://young-reef-89427.herokuapp.com/token","token_endpoint_auth_methods_supported":["none","client_secret_basic","client_secret_jwt","client_secret_post","private_key_jwt"],"token_endpoint_auth_signing_alg_values_supported":["HS256","HS384","HS512","RS256","RS384","RS512","PS256","PS384","PS512","ES256","ES384","ES512"],"userinfo_endpoint":"https://young-reef-89427.herokuapp.com/me","userinfo_signing_alg_values_supported":["none","HS256","HS384","HS512","RS256"],"code_challenge_methods_supported":["plain","S256"],"claim_types_supported":["normal"]}
```

A note, apparently, `lvh.me` is no longer in existance. I need to do something else. The solution should be `nip.io`. Explicitly, I exchanged `http://lvh.me/cb` for `http://127.0.0.1.nip.io:8090`.

## 01 

For part two, I'll keep the ideas, but, in my style (as one does).

Firstly, they add a configuration to the provider initialization. 

This is the result of enabling all those features: 
```
{"authorization_endpoint":"https://young-reef-89427.herokuapp.com/auth","claims_parameter_supported":true,"claims_supported":["auth_time","iss","sub"],"grant_types_supported":["implicit","authorization_code","refresh_token"],"id_token_signing_alg_values_supported":["none","HS256","HS384","HS512","ES256","ES384","ES512","RS256","RS384","RS512","PS256","PS384","PS512"],"issuer":"https://young-reef-89427.herokuapp.com","jwks_uri":"https://young-reef-89427.herokuapp.com/certs","registration_endpoint":"https://young-reef-89427.herokuapp.com/reg","request_object_signing_alg_values_supported":["none","HS256","HS384","HS512","RS256","RS384","RS512","PS256","PS384","PS512","ES256","ES384","ES512"],"request_parameter_supported":true,"request_uri_parameter_supported":true,"require_request_uri_registration":true,"response_modes_supported":["form_post","fragment","query"],"response_types_supported":["code id_token token","code id_token","code token","code","id_token token","id_token","none"],"scopes_supported":["openid","offline_access"],"subject_types_supported":["public"],"token_endpoint":"https://young-reef-89427.herokuapp.com/token","token_endpoint_auth_methods_supported":["none","client_secret_basic","client_secret_jwt","client_secret_post","private_key_jwt"],"token_endpoint_auth_signing_alg_values_supported":["HS256","HS384","HS512","RS256","RS384","RS512","PS256","PS384","PS512","ES256","ES384","ES512"],"userinfo_endpoint":"https://young-reef-89427.herokuapp.com/me","userinfo_signing_alg_values_supported":["none","HS256","HS384","HS512","ES256","ES384","ES512","RS256","RS384","RS512","PS256","PS384","PS512"],"code_challenge_methods_supported":["plain","S256"],"introspection_endpoint":"https://young-reef-89427.herokuapp.com/token/introspection","introspection_endpoint_auth_methods_supported":["none","client_secret_basic","client_secret_jwt","client_secret_post","private_key_jwt"],"introspection_endpoint_auth_signing_alg_values_supported":["HS256","HS384","HS512","RS256","RS384","RS512","PS256","PS384","PS512","ES256","ES384","ES512"],"revocation_endpoint":"https://young-reef-89427.herokuapp.com/token/revocation","revocation_endpoint_auth_methods_supported":["none","client_secret_basic","client_secret_jwt","client_secret_post","private_key_jwt"],"revocation_endpoint_auth_signing_alg_values_supported":["HS256","HS384","HS512","RS256","RS384","RS512","PS256","PS384","PS512","ES256","ES384","ES512"],"id_token_encryption_alg_values_supported":["RSA-OAEP","RSA-OAEP-256","RSA1_5","ECDH-ES","ECDH-ES+A128KW","ECDH-ES+A192KW","ECDH-ES+A256KW","A128GCMKW","A192GCMKW","A256GCMKW","A128KW","A192KW","A256KW","PBES2-HS256+A128KW","PBES2-HS384+A192KW","PBES2-HS512+A256KW"],"id_token_encryption_enc_values_supported":["A128CBC-HS256","A128GCM","A192CBC-HS384","A192GCM","A256CBC-HS512","A256GCM"],"userinfo_encryption_alg_values_supported":["RSA-OAEP","RSA-OAEP-256","RSA1_5","ECDH-ES","ECDH-ES+A128KW","ECDH-ES+A192KW","ECDH-ES+A256KW","A128GCMKW","A192GCMKW","A256GCMKW","A128KW","A192KW","A256KW","PBES2-HS256+A128KW","PBES2-HS384+A192KW","PBES2-HS512+A256KW"],"userinfo_encryption_enc_values_supported":["A128CBC-HS256","A128GCM","A192CBC-HS384","A192GCM","A256CBC-HS512","A256GCM"],"request_object_encryption_alg_values_supported":["ECDH-ES","ECDH-ES+A128KW","ECDH-ES+A192KW","ECDH-ES+A256KW","RSA-OAEP","RSA-OAEP-256","RSA1_5"],"request_object_encryption_enc_values_supported":["A128CBC-HS256","A128GCM","A192CBC-HS384","A192GCM","A256CBC-HS512","A256GCM"],"check_session_iframe":"https://young-reef-89427.herokuapp.com/session/check","end_session_endpoint":"https://young-reef-89427.herokuapp.com/session/end","claim_types_supported":["normal"]}
```

Step 05 

```
heroku open '/auth?client_id=foo&response_type=id_token+token&scope=openid&nonce=foobar'
    https://example.com/#id_token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IksyNEYtQi1Wb3R0TzJsbDhLQ0tTbkI1cnlmRG03OGxsNWJpaWZ6aXNSZGsifQ.eyJzdWIiOiJhIiwibm9uY2UiOiJmb29iYXIiLCJhdF9oYXNoIjoiY2ZKbUljMkQyZ0plMGg5WDUzNzBEdyIsImlhdCI6MTUxODI1MTI0NCwiZXhwIjoxNTE4MjU0ODQ0LCJhdWQiOiJmb28iLCJpc3MiOiJodHRwczovL3lvdW5nLXJlZWYtODk0MjcuaGVyb2t1YXBwLmNvbSJ9.Lelvyh64qQ_W5oq8C9RXyEqkLkXIATo3lmw5asoWEqBV9P3W4liWGceYjqIqWNWQjau8R1Yr0PGd5gaKbuhZoXLVq5rgx73L-NUkhcwmEEpZXQitf_K1tuV-pIny9kpJKsXQ1CNMZek3tKPnkGqRq5ZbuDnDsYts8IB5DVlBFJtTHpa1FXIim7waiaAgSyklJwA4Rho4V-eQWBCQXR-GAhZyaSex9yXvcrOGOMfpqpkOixXn29zjlY_0V4qTQS17CNaxe_lefUWT28ru8NaaWHGgOWBam244uUwP6Uw50OjxKL7RFZtVk_fZXSYUFB-v3-qIbn0mDFc0KxKr3KKdog&access_token=MTU1ZDFiZjItNmMzNy00NjU4LThkYmUtYzI4ZWE2YTVjMzQ2Ld477e4MovTwZPYwfyT3_hiBHLYUBKnnP4f1DgMfTbSjxAtBOlcF2ww6gKbD3zVZYY0n-zljbSCI_Kwgp62jOA&expires_in=3600&token_type=Bearer&session_state=278f354671ad9ed7693c5008cfd7e689a875a2aa89085ac8d2be8e7cd4ea1765.49424758439ea7c3

heroku open /me?access_token=MTU1ZDFiZjItNmMzNy00NjU4LThkYmUtYzI4ZWE2YTVjMzQ2Ld477e4MovTwZPYwfyT3_hiBHLYUBKnnP4f1DgMfTbSjxAtBOlcF2ww6gKbD3zVZYY0n-zljbSCI_Kwgp62jOA
    {"sub":"a"}

heroku restart

heroku open /me?access_token=MTU1ZDFiZjItNmMzNy00NjU4LThkYmUtYzI4ZWE2YTVjMzQ2Ld477e4MovTwZPYwfyT3_hiBHLYUBKnnP4f1DgMfTbSjxAtBOlcF2ww6gKbD3zVZYY0n-zljbSCI_Kwgp62jOA
    {"sub":"a"}
```


### 03 

Following step 05 of part 03, we see: after using `heroku open '/auth?client_id=foo&response_type=id_token+token&scope=openid+email&nonce=foobar&prompt=login'`

```
https://example.com/
#id_token=eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IksyNEYtQi1Wb3R0TzJsbDhLQ0tTbkI1cnlmRG03OGxsNWJpaWZ6aXNSZGsifQ.eyJzdWIiOiIyMzEyMWQzYy04NGRmLTQ0YWMtYjQ1OC0zZDYzYTlhMDU0OTciLCJlbWFpbCI6ImZvb0BleGFtcGxlLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJhdXRoX3RpbWUiOjE1MTgyODc1MDQsIm5vbmNlIjoiZm9vYmFyIiwiYXRfaGFzaCI6Il91NjkwNjdfZ01PeVI5Sy00cXM3TWciLCJpYXQiOjE1MTgyODc1MDQsImV4cCI6MTUxODI5MTEwNCwiYXVkIjoiZm9vIiwiaXNzIjoiaHR0cHM6Ly95b3VuZy1yZWVmLTg5NDI3Lmhlcm9rdWFwcC5jb20ifQ.B5Y6bKrIZSHfn5z-0hTWoruAUMguoRqbSx-sWaluoorkMOcaRxCVJomj5Kr4Mk3Tl2XfDcfjKUWKdB9cC4SvU4eP-cYV9cntxSBBPAQBtm6_TWnvZ5gIP-xOtDQKZv3RAfhroNVJ_pITYbb2P_DdM_GOF03Hh5YBcQbM1EodoMC6rQu-OhFhC3GOJ_qfh58Wsf6NJOlSzbqOq1SYM1SuIrD9FrZNqPbLU3jHwn7f4oHSZSROLMnvcohUhBRImi6NgFxqDfycy3id7rGmElXx2r-a14OYTzRxhBlQg65bfxTr60kLdYYgMlcizZwOGhcqGEgwBbc2jx4hWrcV5yjI5A
&access_token=YjA4YmZhNjItZjQyYi00ZTE5LTk3ODMtY2EyZjY5MWZjZWY5_D91wFFSZShby36c8t3FMMY25g2rZ9vTR6nJHzmqClq55sNO9EVxam7kWKsJVFq2i2dNL1hgLE3MwbZyzw3hPg
&expires_in=3600
&token_type=Bearer&session_state=0a32dba568f203cbcb3a29228077ba000dd688cb2665382aef1749bc76c2ce2c.01b1fe2c74930177
```