# Bug repro: navikt/mock-oauth2-server

## Reproduction

Note that I'm only using `localhost` with Docker because it's not relevant to
this reproduction, but I'm using a new hostname (with an entry in `/etc/hosts`
in my actual project).

1. `docker-compose up --detach`
2. `curl --silent -X POST http://localhost:8080/issuer1/token -d @post-data.txt`
3. Copy `id_token` and paste into jwt.io.

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

[Full token][example-token]

```json
{
  "sub": "f39ce79b-2096-4551-b766-b0e0429ba7ea",
  "aud": "bar",
  "nbf": 1652221507,
  "azp": "bar",
  "iss": "http://localhost:8080/issuer1",
  "exp": 1652225107,
  "iat": 1652221507,
  "jti": "59df2229-4d79-4a63-969f-2a4c53c872f3",
  "tid": "issuer1"
}
```

![](https://i.imgur.com/yxDcPWj.png)


   [example-token]: https://jwt.io/?token=eyJraWQiOiJpc3N1ZXIxIiwidHlwIjoiSldUIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiJmMzljZTc5Yi0yMDk2LTQ1NTEtYjc2Ni1iMGUwNDI5YmE3ZWEiLCJhdWQiOiJiYXIiLCJuYmYiOjE2NTIyMjE1MDcsImF6cCI6ImJhciIsImlzcyI6Imh0dHA6XC9cL2xvY2FsaG9zdDo4MDgwXC9pc3N1ZXIxIiwiZXhwIjoxNjUyMjI1MTA3LCJpYXQiOjE2NTIyMjE1MDcsImp0aSI6IjU5ZGYyMjI5LTRkNzktNGE2My05NjlmLTJhNGM1M2M4NzJmMyIsInRpZCI6Imlzc3VlcjEifQ.XIxs-KeO84GXAN7K3N0TRu0V8-qTKvIaOnsV-9ah5X6sBD0pf2dU-5bCXqxIGUO2d6w-E3tL5Hsz56Oj9yXPSER3RjGKQQn_Y6fyAY-dJKe48QBJBYnwMqGB53meP-s9fdFsd80V9y7PiVISbB7P5BTXcfiKrPUitgvtH8lguKadlV7hgSF18_Hu5-7-RacoqiQdqpC1JXdrXfrQwhqIdvjUrnTzydIYzPnYENNrPkMJnRLl9LtZWvcVtKCqC8FT65el704Yb7O_l-jsTrY_6jtmuL1p2g1m_4Kc0sXrZnbm6CGmoCXsGvjRAURK8XPohuO28YK1jShKgIc3KlJTsQ
   [readme-ref]: https://github.com/navikt/mock-oauth2-server#:~:text=return%20a%20token%20response%20containing%20a%20token%20with%20the%20following%20claims
