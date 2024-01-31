Kong command to create a service:

    http POST localhost:8001/services name=mockbin_service  url=http://mockbin:8080/request

Create a Route:
  
    http -f POST localhost:8001/services/mockbin_service/routes name=mockbin_route paths=/mockbin

Create a Rate limit plugin

    http -f POST localhost:8001/services/mockbin_service/plugins name=rate-limiting  config.minute=5  config.policy=local
