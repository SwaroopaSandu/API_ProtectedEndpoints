# API_ProtectedEndpoints



#### INSTALL DEPENDENCIES
```
mix deps.get
```


To create protected endpoints in your Elixir REST API project using Guardian, you can follow these steps:

1. Add Guardian as a Dependency:
   - In your project's `mix.exs` file, add `:guardian` as a dependency and run `mix deps.get` to fetch the dependency.
   - Guardian provides authentication and token generation functionality.

2. Configure Guardian:
   - Create a configuration file (e.g., `config/guardian.exs`) to define your Guardian configuration.
   - Specify the secret key used for token signing, token expiration time, and any other desired settings.
   - Example configuration:
     ```elixir
     use Mix.Config

     config :my_app, MyApp.Guardian,
       allowed_algos: ["HS512"],
       secret_key: "my_secret_key",
       issuer: "MyApp",
       ttl: {30, :days}
     ```

3. Implement a Guardian Pipeline Module:
   - Create a pipeline module that will use Guardian plugs to verify authentication tokens for protected endpoints.
   - The pipeline module defines a pipeline with one or more plugs that will be executed before reaching the protected endpoint.
   - Example pipeline module:
     ```elixir
     defmodule MyApp.AuthenticationPipeline do
       use Guardian.Plug.Pipeline,
         otp_app: :my_app,
         module: MyApp.Guardian

       plug Guardian.Plug.VerifyHeader, realm: "Bearer"
       plug Guardian.Plug.EnsureAuthenticated
     end
     ```

4. Apply the Guardian Pipeline to Protected Endpoints:
   - In your router file (e.g., `lib/my_app_web/router.ex`), apply the Guardian pipeline to the desired endpoints or router scopes.
   - The pipeline ensures that the request contains a valid authentication token before allowing access to the protected endpoint.
   - Example usage in a router scope:
     ```elixir
     pipeline :api do
       plug :accepts, ["json"]
       plug MyApp.AuthenticationPipeline
     end

     scope "/api", MyAppWeb do
       pipe_through :api

       # Protected endpoints go here
       # ...
     end
     ```

5. Send Requests with Bearer Token:
   - To access the protected endpoints, clients should include an `Authorization` header in their requests with the value `Bearer <token>`.
   - Replace `<token>` with a valid authentication token obtained through the authentication process.

6. Return Account Object if Authenticated:
   - Inside the protected endpoint's controller or handler, you can access the authenticated user's account object and return it as needed.
   - The user's account object is typically available in the connection's `current_user` field.
   - Example handler returning the account object:
     ```elixir
     def index(conn, _params) do
       account = Guardian.Plug.current_resource(conn)
       render(conn, %{account: account})
     end
     ```

By following these steps, you can use Guardian to create protected endpoints in your Elixir REST API project. The Guardian pipeline verifies the authentication token, ensuring that only authenticated requests can access the protected endpoints, and allows you to retrieve the account object associated with the authenticated user.
