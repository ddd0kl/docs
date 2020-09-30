---
title: SCIM
redirect_from:
  - /v3/scim
versions:
  free-pro-team: '*'
---

### SCIM Provisioning for Organizations

The SCIM API is used by SCIM-enabled Identity Providers (IdPs) to automate provisioning of {% data variables.product.product_name %} organization membership. A API {% data variables.product.product_name %} é baseada na versão 2.0 do [padrão SCIM](http://www.simplecloud.info/). The {% data variables.product.product_name %} SCIM endpoint that an IdP should use is: `{% data variables.product.api_url_code %}/scim/v2/organizations/{org}/`.

{% note %}

**Note:** The SCIM API is available only to organizations on [{% data variables.product.prodname_ghe_cloud %}](/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-accounts) with [SAML SSO](/v3/auth/#authenticating-for-saml-sso) enabled. Para obter mais informações sobre o SCIM, consulte "[Sobre o SCIM](/github/setting-up-and-managing-organizations-and-teams/about-scim)."

{% endnote %}

### Authenticating calls to the SCIM API

You must authenticate as an owner of a {% data variables.product.product_name %} organization to use its SCIM API. A API espera que um token [OAuth 2.0](/developers/apps/authenticating-with-github-apps) seja incluído no cabeçalho da `Autorização`. Você também pode usar um token de acesso pessoal, mas primeiro deve [autorizá-lo para uso com sua organização SAML SSO](/github/authenticating-to-github/authorizing-a-personal-access-token-for-use-with-saml-single-sign-on).

### Mapping of SAML and SCIM data

O SAML IdP e o cliente SCIM devem usar valores correspondentes ao `NameID` e `userName` para cada usuário. Isso permite que um usuário que faz autenticação através do SAML seja vinculado à sua identidade SCIM provisionada.

### Supported SCIM User attributes

| Nome             | Tipo      | Descrição                                                                                                                                                                                                                                                                                                                                                                                        |
| ---------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `userName`       | `string`  | The username for the user.                                                                                                                                                                                                                                                                                                                                                                       |
| `name.givenName` | `string`  | The first name of the user.                                                                                                                                                                                                                                                                                                                                                                      |
| `name.lastName`  | `string`  | The last name of the user.                                                                                                                                                                                                                                                                                                                                                                       |
| `emails`         | `array`   | List of user emails.                                                                                                                                                                                                                                                                                                                                                                             |
| `externalId`     | `string`  | This identifier is generated by the SAML provider, and is used as a unique ID by the SAML provider to match against a GitHub user. You can find the `externalID` for a user either at the SAML provider, or using the [List SCIM provisioned identities](#list-scim-provisioned-identities) endpoint and filtering on other known attributes, such as a user's GitHub username or email address. |
| `id`             | `string`  | Identifier generated by the GitHub SCIM endpoint.                                                                                                                                                                                                                                                                                                                                                |
| `ativo`          | `boolean` | Used to indicate whether the identity is active (true) or should be deprovisioned (false).                                                                                                                                                                                                                                                                                                       |

{% note %}

**Observação:** As URLs de Endpoint para a API SCIM são sensíveis a maiúsculas e minúsculas. Por exemplo, a primeira letra no endpoint `Usuários` deve ser maiúscula:

```shell
GET /scim/v2/organizations/{org}/Users/{scim_user_id}
```

{% endnote %}

{% include rest_operations_at_current_path %}