# 💻 Gum GraphQL Queries

The Gum SDK is a client library that provides a convenient way to access the GraphQL API and simplify the process of making GraphQL queries.

### Initialise the SDK with graphQL client

{% code overflow="wrap" %}
```typescript
import { AnchorWallet, useAnchorWallet } from '@solana/wallet-adapter-react';
import { GRAPHQL_ENDPOINTS, useGum } from '@gumhq/react-sdk';
import { useConnection } from '@solana/wallet-adapter-react';
import { useMemo } from 'react';
import { GraphQLClient } from "graphql-request";

export const useGumSDK = () => {
  const { connection } = useConnection();
  const anchorWallet = useAnchorWallet() as AnchorWallet;

  // GraphQL endpoint is chosen based on the network
  const graphqlEndpoint = GRAPHQL_ENDPOINTS['devnet'];

  const gqlClient = useMemo(() => new GraphQLClient(graphqlEndpoint), [graphqlEndpoint]);

  const sdk = useGum(anchorWallet, connection, {preflightCommitment: 'confirmed'}, "devnet", gqlClient);

  return sdk;
};
```
{% endcode %}

### Access GraphQL Functions

Once you have initialised the SDK, you can start accessing the GraphQL functions. The Gum SDK provides a `gqlClient` object that you can use to send GraphQL requests.&#x20;

Here is an example of how to make a GraphQL query using the `gqlClient`  to fetch all profiles:&#x20;

<pre class="language-typescript"><code class="lang-typescript">sdk.gqlClient.request(`
<strong>  query AllProfiles {
</strong>    profile {
      address
      screen_name
      authority
      metadata_uri
      metadata
      slot_created_at
      slot_updated_at
    }
  }
`).then((result) => {
  console.log(result);
});
</code></pre>

You can find reference implementations for using the GraphQL functions in the Gum SDK on our Github repository at this link: [https://github.com/gumhq/sdk/blob/master/packages/gpl-core/src/profile.ts#L69-L137](https://github.com/gumhq/sdk/blob/master/packages/gpl-core/src/profile.ts#L69-L137).&#x20;

These implementations can be used as a starting point for your projects or as a reference as you build your own GraphQL queries.

### Testing Queries with the Playground

#### Public API Endpoints

The public API endpoints can be used to test GraphQL queries in the Playground.

{% tabs %}
{% tab title="Devnet" %}
[https://cloud.hasura.io/public/graphiql?endpoint=https://gum-indexer-smartprofile-devnet-lafkve5tyq-uc.a.run.app/v1/graphql](https://cloud.hasura.io/public/graphiql?endpoint=https://gum-indexer-smartprofile-devnet-lafkve5tyq-uc.a.run.app/v1/graphql)
{% endtab %}

{% tab title="Mainnet" %}
[https://cloud.hasura.io/public/graphiql?endpoint=https://gum-indexer-smartprofile-mainnet-lafkve5tyq-uc.a.run.app/v1/graphql](https://cloud.hasura.io/public/graphiql?endpoint=https://gum-indexer-smartprofile-mainnet-lafkve5tyq-uc.a.run.app/v1/graphql)
{% endtab %}
{% endtabs %}
