Kong command to create a service:

    http POST localhost:8001/services name=mockbin_service  url=http://mockbin:8080/request

Create a Route:
  
    http -f POST localhost:8001/services/mockbin_service/routes name=mockbin_route paths=/mockbin

Create a Rate limit plugin

    http -f POST localhost:8001/services/mockbin_service/plugins name=rate-limiting  config.minute=5  config.policy=local

Create a Key-Auth plugin

    http POST localhost:8001/services/mockbin_service/plugins name=key-auth

Create a Consumer and assign credentials
    
    http POST localhost:8001/consumers username=Jane

Assign Credentials to consumer

    http POST localhost:8001/consumers/Jane/key-auth key=JanePassword

Call API by passing consumer credentials key-auth

    http -h GET localhost:8000/mockbin/request?apikey=JanePassword

Call API recursively - generate traffic

    for ((i=1;i<=10;i++))
      do
      echo Request $i
      http -h GET localhost:8000/mockbin/request?apikey=JanePassword
      sleep 1
    done

Add JWT plugin - service level

    http POST localhost:8001/services/mockbin_service/plugins name=jwt

Assign JWT credentials to Consumer

    http POST localhost:8001/consumers/Jane/jwt

Get JWT Key/Secret for consumer and Generate JWT Token

    KEY=$(http GET localhost:8001/consumers/Jane/jwt | jq '.data[0].key' | xargs)
    SECRET=$(http GET localhost:8001/consumers/Jane/jwt | jq '.data[0].secret'|xargs)
    TOKEN=$(jwt encode --iss $KEY -S $SECRET)

Consuming the service with JWT credentials:

    http -h GET localhost:8000/mockbin Authorization:"Bearer $TOKEN"
    


    
