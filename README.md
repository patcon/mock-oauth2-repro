# Bug repro: navikt/mock-oauth2-server

## Reproduction

1. Add this to your `/etc/hosts` file:
    ```
    127.0.0.1 mock-auth.test
    ```

2. `docker-compose up --detach`
3. `curl --silent -X POST http://mock-auth.test:8080/issuer1/token -d @post-data.txt`
4. Copy `id_token` and paste into jwt.io.

## Expected Result

[As per the README][readme-ref], I would expect a token returned in the current format

```json
{
  "sub": "subByScope",
  "aud": "audByScope",
  "nbf": 1616416942,
  "iss": "http://localhost:54905/issuer1",
  "exp": 1616417062,
  "iat": 1616416942,
  "jti": "28697333-6f25-4b1f-b2c2-409ce010933a"
}
```

## Observed Result

Instead I get a token like this:

```json
{
  "sub": "6e452b69-1edb-467e-b7bb-a15235b4f390",
  "aud": "bar",
  "nbf": 1652220339,
  "azp": "bar",
  "iss": "http://mock-auth.test:8080/issuer1",
  "exp": 1652223939,
  "iat": 1652220339,
  "jti": "9b647f4a-247b-4630-9636-e462eab93212",
  "tid": "issuer1"
}
```

[Example token][example-token]

![](https://i.imgur.com/J7Z2eza.png)


   [example-token]: https://jwt.io/?token=eyJraWQiOiJpc3N1ZXIxIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiI2ZTQ1MmI2OS0xZWRiLTQ2N2UtYjdiYi1hMTUyMzViNGYzOTAiLCJhdWQiOiJiYXIiLCJuYmYiOjE2NTIyMjAzMzksImF6cCI6ImJhciIsImlzcyI6Imh0dHA6XC9cL21vY2stYXV0aC50ZXN0OjgwODBcL2lzc3VlcjEiLCJleHAiOjE2NTIyMjM5MzksImlhdCI6MTY1MjIyMDMzOSwianRpIjoiOWI2NDdmNGEtMjQ3Yi00NjMwLTk2MzYtZTQ2MmVhYjkzMjEyIiwidGlkIjoiaXNzdWVyMSJ9.agzy9GMhidf_pbkbJm5pYYPgPMVdtCMzraDtieL298bAzPChILYXhs9KW_eV7jTAv6cEGBUM_ijRAR5w6fPmUBY1gWoPcBQdvRRgTssnEWpXl8riuBJS1z_IIdCXCu5jZ27x-fqxr5W4FmcNaM2vbv0p6ytpO0P36gqiHlODVgZEHGhPWOz1oWayHzZumQrjxsVN3SHkaCyh3c6rW2VB44w6TKxltLAZbtL_-pmaH5cRqJX9KXtwbd92v3AfWqhFnfP7xbEiKAzxPDNnZypyy2crxukKRy3s7CfSMUaBHFm6lYI2Wp34mRUwtGU0btncxWn4kvAICqQlZM52xg8CGg
   [readme-ref]: https://github.com/navikt/mock-oauth2-server#:~:text=return%20a%20token%20response%20containing%20a%20token%20with%20the%20following%20claims
