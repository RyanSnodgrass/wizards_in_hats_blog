# Webservices Notes

in Webservicess::Authorization.validate_token:
``` ruby
def validate_token(token:, config:)
      return :ok if config.bypass_key_validation?
      return :unauthorized unless token.source.present?

      backend = config.api_authorization_service
      config.logger.debug("API Authorization back-end: #{backend}")
      backend.call(token: token)
    end
```

`config` is a class defined within the same file.

```ruby
def api_authorization_service
  return app_config.webservices_backend if app_config.respond_to? :webservices_backend

  ::Webservices::Backend::ThreeScale
end
```
