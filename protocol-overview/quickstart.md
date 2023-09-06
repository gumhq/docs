# ⚡ Quickstart

## Prerequisites

Before interacting with the Gum program library, make sure to install the Solana web3.js and wallet adapter libraries. Run the following commands to add them to your project:

```bash
yarn add @solana/web3.js
yarn add @solana/wallet-adapter-base @solana/wallet-adapter-react @solana/wallet-adapter-react-ui @solana/wallet-adapter-wallets
```

## Installation

To get the Gum React SDK, simply install it using the following command:

```bash
yarn add @gumhq/react-sdk
```

### Initialise a SDK

To get started, create a new hook `useGumSDK.ts` file and import the necessary modules:

```typescript
/* src/hooks/useGumSDK.ts */
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

The code snippet above initializes the Gum SDK and connects to the necessary services.

### Providers

In your application, three key higher-order components (providers) are used to manage the Gum SDK, session wallet, and uploading services:

1. **GumProvider**: Provides the Gum SDK context across your app.
2. **SessionWalletProvider**: Offers the sessionWallet context throughout your app.
3. **UploaderProvider**: Makes the `useUploaderContext` accessible across the application, simplifying metadata uploads.

Set up these providers by creating a `GumSDKProvider.tsx` file:

<pre class="language-typescript"><code class="lang-typescript"><strong>// components/GumSDKProvider.tsx
</strong><strong>// The GumSDKProvider component initializes the Gum SDK and SessionKeyManager and provides it to its children via context.
</strong>
import { GumProvider, SessionWalletProvider, UploaderProvider, useSessionKeyManager } from '@gumhq/react-sdk';
import { AnchorWallet, useAnchorWallet, useConnection } from '@solana/wallet-adapter-react';
import { useGumSDK } from '@/hooks/useGumSDK';

interface GumSDKProviderProps {
  children: React.ReactNode;
}

const GumSDKProvider: React.FC&#x3C;GumSDKProviderProps> = ({ children }) => {
  const { connection } = useConnection();
  const anchorWallet = useAnchorWallet() as AnchorWallet;
  const sdk = useGumSDK();
  const sessionWallet = useSessionKeyManager(anchorWallet, connection, "devnet");

  if (!sdk) {
    return null;
  }

  return (
    &#x3C;GumProvider sdk={sdk}>
      &#x3C;SessionWalletProvider sessionWallet={sessionWallet}>
        &#x3C;UploaderProvider
            uploaderType="arweave"
            connection={connection}
            cluster="devnet"
          >
            {children}
        &#x3C;/UploaderProvider>
      &#x3C;/SessionWalletProvider>
    &#x3C;/GumProvider>
  );
};

export default GumSDKProvider;
</code></pre>

Additionally, create a `WalletContextProvider.tsx` file to setup the wallet adapter in your dApp:

<pre class="language-typescript"><code class="lang-typescript"><strong>// contexts/WalletContextProvider.tsx
</strong><strong>import { Adapter, WalletAdapterNetwork, WalletError } from '@solana/wallet-adapter-base';
</strong>import { ConnectionProvider, WalletProvider } from '@solana/wallet-adapter-react';
import { FC, ReactNode, useCallback } from 'react';
import dynamic from "next/dynamic";

export const ReactUIWalletModalProviderDynamic = dynamic(
  async () =>
    (await import("@solana/wallet-adapter-react-ui")).WalletModalProvider,
  { ssr: false }
);

export const WalletContextProvider: FC&#x3C;{ children: ReactNode, endpoint: string, network: WalletAdapterNetwork, wallets?: Adapter[] }> = ({ children, endpoint, network, wallets = [] }) => {

    const onError = useCallback(
        (error: WalletError) => {
            console.error(error);
        },
        []
    );

    return (
        &#x3C;ConnectionProvider endpoint={endpoint}>
            &#x3C;WalletProvider wallets={wallets} onError={onError} autoConnect>
                &#x3C;ReactUIWalletModalProviderDynamic>
                    {children}
                &#x3C;/ReactUIWalletModalProviderDynamic>
			&#x3C;/WalletProvider>
        &#x3C;/ConnectionProvider>
    );
};
</code></pre>

Ensure that the Gum SDK context is available across all your components by wrapping `GumSDKProvider` around your entire app in the `_app.tsx` file:

```typescript
// _app.tsx
import { WalletContextProvider } from '@/contexts/WalletContextProvider'
import '@/styles/globals.css'
import type { AppProps } from 'next/app'
import { WalletAdapterNetwork } from '@solana/wallet-adapter-base'
import { PhantomWalletAdapter } from '@solana/wallet-adapter-wallets'
import { clusterApiUrl } from '@solana/web3.js'
import { useMemo } from 'react'
import GumSDKProvider from '@/components/GumSDKProvider'
import dotenv from 'dotenv'

dotenv.config()
// Use require instead of import since order matters
require('@solana/wallet-adapter-react-ui/styles.css');

export default function App({ Component, pageProps }: AppProps) {
  const network = WalletAdapterNetwork.Devnet;
  const endpoint = process.env.NEXT_PUBLIC_SOLANA_ENDPOINT || clusterApiUrl(network);
  const wallets = useMemo(
      () => [
          new PhantomWalletAdapter(),
      ],
      [network]
  );

  return (
    <WalletContextProvider endpoint={endpoint} network={network} wallets={wallets} >
      <GumSDKProvider> 
          <Component {...pageProps} />
      </GumSDKProvider>
    </WalletContextProvider>
  )
}
```

### Utilising the SDK

Once the `GumSDKProvider` is initialised, it can be used in your React components. You can import the required modules and call the hook as shown below:

```typescript
import { useGumContext, useSessionWallet, useUploaderContext } from "@gumhq/react-sdk";

const ExampleComponent = () => {
  // Utilize the various contexts
  const { sdk } = useGumContext();
  const session = useSessionWallet();
  const { publicKey: sessionPublicKey, sessionToken, createSession, sendTransaction }  = session;
  const { handleUpload, uploading, error } = useUploaderContext();

  // You would typically use these in some way inside your component
  // For instance, you could create a function that utilizes these and call that function based on some event (like a button click)

  return (
    // This is where your HTML goes
    // You can use the variables above to populate the data in your HTML
    // Example:
    // <div>{sessionPublicKey}</div>
    <div>Your HTML goes here</div>
  );
};

export default ExampleComponent;
```

The given code demonstrates how to access various contexts provided by the Gum SDK in your React components. You can use these in your functions or events and populate data in your HTML accordingly.
