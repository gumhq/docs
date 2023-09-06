# 🎖 Issue Badge

Developers have the ability to issue Badges to users, providing a public, on-chain identification for various reasons such as signifying an achievement, status, or affiliation. Internally, we use the [Gateway](https://www.mygateway.xyz/) to issue these badges. Here are the steps to issue badges to your users:

### Create an Issuer Account

To issue a badge, you must first create an Issuer account. Use the `createIssuer()` function in your React component as follows:

```typescript
const handleCreateIssuer = async () => {
  const res = await createIssuer(publicKey);
  console.log(`Issuer account: ${res}`);
}
```

### Verification of the Issuer Account

After creating the Issuer account, reach out to us on Discord with the Issuer account address. We will verify your Issuer status, and once approved, you will be able to issue badges to your users.

### Create a Schema Account

We provide a pre-existing schema that you can use if you want to issue badges marking your users as affiliated with or a part of your app.

Here are the details:

Devnet Schema Account: `JC2DD8R1CN5ixjB2iscbuKtg83EdxSotKfkFTApzheCB` Schema MetadataUri: `https://arweave.net/fSgTP6jg_hStbA8nTtJYmid4WBWq5JrZPMJI6p3Dtu4`

However, if your use-case requires a different schema, you would need to create a new one. For such cases, please contact us for further instructions. You can create a schema using the `createSchema()` function:

```typescript
const handleCreateSchema = async () => {
  const schema = await sdk.badge.createSchema(metadataUri, publicKey);
  console.log(`Schema account: ${schema}`);
}
```

### Issue a Badge

Once your Issuer account is created and verified, and a Schema account is set up, you can issue badges to your users using the `issueGatewayBadge()` function:

```typescript
const handleIssueBadge = async () => {
  const holderProfile = new PublicKey("Your_Holder_Profile_PublicKey");
  const schemaAccount = new PublicKey("JC2DD8R1CN5ixjB2iscbuKtg83EdxSotKfkFTApzheCB");
  const issuerAccount = new PublicKey("Your_Issuer_Account_PublicKey");
  const gumTldAccount = new PublicKey("Your_GumTLD_Account_PublicKey");
  const badge = await issueGatewayBadge(wallet, publicKey, holderProfile, schemaAccount, issuerAccount, gumTldAccount, "app_name", "joining_date", publicKey, publicKey);
  console.log(`Badge: ${badge}`);
}
```

Please replace the placeholders with your actual data.

To make the process more understandable, here is an example code snippet demonstrating how you can integrate these steps within a React component using the `@gumhq/react-sdk`:

```typescript
import { useState } from 'react';
import { useBadge, useGumContext, SDK } from '@gumhq/react-sdk';
import { useWallet } from '@solana/wallet-adapter-react';
import { PublicKey } from '@solana/web3.js';

const BadgeIssuerTemplateComponent = () => {
  const wallet = useWallet();
  const { publicKey } = wallet;
  const { sdk } = useGumContext() as { sdk: SDK };
  const { createBadge, issueGatewayBadge, createIssuer, isProcessing, processingError } = useBadge(sdk);

  const [metadata, setMetadata] = useState('');

  return (
    <div>
      <h1>Create Issuer</h1>
      {publicKey && (
        <button
          onClick={async () => {
            const res = await createIssuer(publicKey)
            console.log(`issuer: ${res}`);
          }}
        >
          Create Issuer
        </button>
      )}
      <h1>Create Schema</h1>
      <input
        type="text"
        value={metadata}
        onChange={(event) => setMetadata(event.target.value)}
        placeholder="Enter metadata uri"
      />
      {publicKey && (
        <button
          onClick={async () => {
            const schema = await sdk.badge.createSchema(metadata, publicKey);
            console.log(`schema: ${schema}`);
          }}
        >
          Create Schema
        </button>
      )}
      <h1>Issue Badge</h1>
      {publicKey && (
        <button
          onClick={async () => {
            const holderProfile = new PublicKey("Your_Holder_Profile_PublicKey");
            const schemaAccount = new PublicKey("Your_Schema_Account_PublicKey");
            const issuerAccount = new PublicKey("Your_Issuer_Account_PublicKey");
            const gumTldAccount = new PublicKey("Your_GumTLD_Account_PublicKey");
            const badge = await issueGatewayBadge(wallet, publicKey, holderProfile, schemaAccount, issuerAccount, gumTldAccount, "app_name", "joining_date", publicKey, publicKey);
            console.log(`badge: ${badge}`);
          }}
        >
          Issue Badge
        </button>
      )}
    </div>
  );
};

export default BadgeIssuerTemplateComponent;
```

The verification of the issuer account is gated and needs our approval. Therefore, it's important to reach out to us after creating your Issuer account.

### Querying Badges

After issuing badges, you can retrieve them for verification or display purposes using functions that fetch credentials via Issuer Gateway ID, Credential ID, or User Wallet.

You can find the detailed implementation of these functions in the Gum SDK GitHub repository [here](https://github.com/gumhq/sdk/blob/smart\_profile/packages/gpl-core/src/badge.ts#L191-L231).
