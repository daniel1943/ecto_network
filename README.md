# EctoNetwork
[![Build Status](https://travis-ci.org/adam12/ecto_network.svg?branch=master)](https://travis-ci.org/adam12/ecto_network)

Ecto types to support MACADDR and Network extensions provided by Postgrex.

Although this is primarily an Ecto library, it has a hard dependency on Postgrex
due to the types it is providing.

## Installation

1. Add `ecto_network` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [{:ecto_network, "~> 0.1.0"}]
    end
    ```

2. Add Postgrex extensions for MACADDR and/or Network (INET, CIDR) to your Repo
   config.

    For Phoenix, you will want to update your environment-specific config files
    (`config/dev.exs`, `config/test.exs`, `config/prod.exs`) and add the
    extensions to the your Repo config.

    ```elixir
    # Configure your database
    config :your_app, YourApp.Repo,
      adapter: Ecto.Adapters.Postgres,
      extensions: [
        {Postgrex.Extensions.MACADDR, nil},
        {Postgrex.Extensions.Network, nil}
      ],
      username: "",
      password: "",
      database: "yourapp_dev",
      hostname: "localhost",
      pool_size: 10
    ```

3. Create your migrations using the Postgres types as atoms.

    ```elixir
    def change do
      create table(:your_table) do
        add :ip_address, :inet
        add :mac_address, :macaddr
        add :network, :cidr
      end
    end
    ```

4. Use the new types in your Ecto schemas.

    ```elixir
    schema "your_table" do
      field :ip_address, EctoNetwork.INET
      field :mac_address, EctoNetwork.MACADDR
      field :network, EctoNetwork.CIDR
    end
    ```
