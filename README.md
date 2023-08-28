# oauth2-accesstoken-golang
utility to authenticate and receive oauth2 access token in golang

I built this tool to serve a few purposes:
- a command line tool to authenticate and acquire an oauth2 access token.  This is useful for building oauth2-authenticated programs for personal use that need to do this only once, or when developing an oauth-authenticated application and you need a token.  Such a tool may exist elsewhere but I couldn't find one quickly.
- Demonstrate how to perform oauth2 authentication in golang (and give me a chance to learn some golang along the way).

## Use
copy oauth_client_config.json.template to oauth_client_config.json.  oauth_client_config.json is in .gitignore to avoid commiting secrets.  It is a json representation of oauth2.Config so please see that for more details.  It contains an oauth2.Endpoint.  See https://pkg.go.dev/golang.org/x/oauth2/endpoints for a list of popular endpoints.  Add the necessary id, secret, scopes, endpoint, and redirect URL (should likely stay localhost:8080) to this config.

A quick note on Scopes: some oauth implementations allow, or even expect, comma-delimited scopes.  This is incorrect according to the oauth2 spec, which expects space-delimited.  If you receive a scopes error from the authorization server when requesting multiple scopes then try to instead send a single scope containing all scopes delimited by commas, i.e., `"Scopes": ["scope1,scope2,scope3"]`.

accesstoken logs to accesstoken.log (in .gitignore) and writes the resulting access token to token.txt as json (also in .gitignore).  This token may be unmarshalled back into a token, from which an http client can be created as `client := conf.Client(context.Background(), token)`.  This client will automatically authorize requests and refresh the token as needed.  You will likely want to pass this client to a service api object.