https://vault.ci-accor.io/ui/vault/init

docker-compose exec vault bash

bash-5.0# vault operator init
Unseal Key 1: JkBSItRyl6qjc4av2nI59tL5TEFub5quXg45C/4iRYsC
Unseal Key 2: CKQzakcnsvrAl6EluhDSVzt4/iR0K+qN4zDCZVVyE0PL
Unseal Key 3: AtrHrQ87P23fu40n2lwrhBT7t/nYwJ5Wh5d0Wluy1/Td
Unseal Key 4: NqOTt56aZjW8u35/17jEf+v3Qeeq1cRTcEpvPJA7e7my
Unseal Key 5: OpnSBZu5Wbp3jlrC0jIiC9zTM8OZOIG66Xl2miZE3bwr

Initial Root Token: s.hQtqH5RKoyinYMgvBZsPcZjo

Vault initialized with 5 key shares and a key threshold of 3. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 3 key to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.

vault operator unseal AtrHrQ87P23fu40n2lwrhBT7t/nYwJ5Wh5d0Wluy1/Td
vault operator unseal NqOTt56aZjW8u35/17jEf+v3Qeeq1cRTcEpvPJA7e7my
vault operator unseal OpnSBZu5Wbp3jlrC0jIiC9zTM8OZOIG66Xl2miZE3bwr

authentication

Authentication

»
Via the CLI

The default path is /github. If this auth method was enabled at a different path, specify -path=/my-path in the CLI.

$ vault login -method=github token="MY_TOKEN"
»
Via the API

The default endpoint is auth/github/login. If this auth method was enabled at a different path, use that value instead of github.

$ curl \
    --request POST \
    --data '{"token": "MY_TOKEN"}' \
    http://127.0.0.1:8200/v1/auth/github/login
The response will contain a token at auth.client_token:

{
  "auth": {
    "renewable": true,
    "lease_duration": 2764800,
    "metadata": {
      "username": "my-user",
      "org": "my-org"
    },
    "policies": [
      "default",
      "dev-policy"
    ],
    "accessor": "f93c4b2d-18b6-2b50-7a32-0fecf88237b8",
    "client_token": "1977fceb-3bfa-6c71-4d1f-b64af98ac018"
  }
}