Add OIDC plugin - service level with IDP Keylock

    http -f POST localhost:8001/routes/my-oidc-route/plugins \
    name=openid-connect \
    config.issuer=$KEYCLOAK_CONFIG_ISSUER \
    config.client_id=kong \
    config.client_secret=$CLIENT_SECRET \
    config.redirect_uri=$KEYCLOAK_REDIRECT_URI/oidc \
    config.response_mode=form_post \
    config.ssl_verify=false \
    config.introspection_check_active=false

Different Authentication Types

    Password Grant
    Client credentials
    Authorization code
    bearer token

How to retrieve value from JSON data

    OIDC_PLUGIN_ID=$(http GET localhost:8001/routes/my-oidc-route/plugins/ \
              | jq -r '.data[] | select(.name == "openid-connect") | .id')
