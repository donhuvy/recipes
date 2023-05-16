## Fiber as an Envoy External Authorization HTTP Service

One way of extending the popular [Envoy](https://www.envoyproxy.io) proxy is by developing an
[external authorization service](https://www.envoyproxy.io/docs/envoy/latest/api-v3/service/auth/v3/external_auth.proto).

This example illustrates using `fiber` and `gofiber/keyauth` as an authorization service for a front
proxy (the configuration could also be used for an L2 / Sidecar proxy). See `authz`.

It also uses `fiber` as a sample upstream service, with the following endpoints. See `app`.

### Endpoints

| Name      | Rute          | Protected | Method |
| --------- | ------------- | --------- | ------ |
| Health    | /health       | No        | GET    |
| Resource  | /api/resource | Yes       | GET    |

### Run

`docker-compose up --build -d`

### Test

| Name            | Command                                                           | Status |
| --------------- | ----------------------------------------------------------------- | ------ |
| Not protected   | `curl localhost:8000/health -i`                                   | 200    |
| Missing API key | `curl localhost:8000/api/resource -i`                             | 403    |
| Invalid API key | `curl localhost:8000/api/resource -i -H "x-api-key: invalid-key"` | 403    |
| Valid API key   | `curl localhost:8000/api/resource -i -H "x-api-key: valid-key"`   | 200    |

### Stop

`docker-compose down`
