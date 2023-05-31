# API_ProtectedEndpoints




API_ProtectedEndpoints is an Elixir project that demonstrates how to create protected endpoints for a RESTful API for authentication.

## Installation

  Clone the repository:
  
   git clone https://github.com/SwaroopaSandu/API_ProtectedEndpoints.git
   
#### INSTALL DEPENDENCIES
```
mix deps.get
```   

To create protected endpoints in Elixir, you can use the Guardian library along with Elixir's Plug library. Guardian provides authentication and token generation functionality, while Plug helps with request handling and middleware.

Here's an example of how to create protected endpoints using Guardian and Plug:

1. Add Guardian as a dependency in your project's `mix.exs` file:
   ```elixir
   defp deps do
     [
       {:guardian, "~> 2.0"},
       # Other dependencies...
     ]
   end
   ```

2. Configure Guardian:
   Create a configuration file (e.g., `config/config.exs`) to define your Guardian configuration options. This includes setting the secret key used for token signing, token expiration time, and other settings.
   ```elixir
   
   config :real_deal_api, RealDealApiWeb.Auth.Guardian,
   issuer: "real_deal_api",
   secret_key: "A2QhoBW5+qU4F79ac7Ozo4fUlRpzkeHOYORgJkCazWjvOH22e3esAjryekV/+5Qs"
   
   ```

3. Implementing a Guardian Pipeline Module:
   Create a pipeline module that uses Guardian plugs to verify authentication tokens for protected endpoints. The pipeline module defines a pipeline with one or more plugs that will be executed before reaching the protected endpoint.
   

4. Apply the Guardian Pipeline to Protected Endpoints:
   In your router module (e.g., `lib/real_deal_api_web/router.ex`), apply the Guardian pipeline to the desired endpoints or router scopes. The pipeline ensures that the request contains a valid authentication token before allowing access to the protected endpoint.
  


By following these steps, you can create protected endpoints in your Elixir application using Guardian and Plug. The Guardian pipeline verifies the authentication token and ensures that only authenticated requests can access the protected endpoints. You can customize the behavior of the pipeline, handle authentication errors, and define additional middleware or routes as required.
