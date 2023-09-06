# ❓ FAQs

#### **I'm building on Gum but I'm facing some issues. Where can I reach out?**

The best place to reach out is our discord channel here. Pop a question in the #dev-questions on our [discord](https://discord.gg/tCswbSK5W2) and the team will help you out.

#### How do I get started with building on Gum?

You can get started by downloading our open-source SDK. Check out our quickstart guide here:

{% content-ref url="../protocol-overview/quickstart.md" %}
[quickstart.md](../protocol-overview/quickstart.md)
{% endcontent-ref %}

#### Where can I find the Gum API?

You can find the API [here](https://github.com/gumhq/sdk/tree/master/packages/gpl-core).

#### How do users connect with each other?

{% content-ref url="../concepts/connection.md" %}
[connection.md](../concepts/connection.md)
{% endcontent-ref %}

#### Where is the data stored?

Gum is agonistic to storage provider, just like how NFT (Metaplex) TokenMetadata point to a metadata\_uri. The metadata\_uri can be a reference to any URL (Arweave, IPFS, Genesysgo) as long as they adhere to the following schema.\


```typescript
class PostMetadata {
  content: Blocks | Image | Video | Json | Text;
  type: "blocks" | "image" | "video" | "json" | "text";
  image_preview?: string;
  text_preview?: string;
  authorship: {
    signature: string;
    publicKey: string;
  }
  app_id: string;
  contentDigest?: string;
  signatureEncoding?: string;
  digestEncoding?: string;
  parentDigest?: string;
}
```

However, note that our indexers fetch the URL, validate the response and store them, so that it is lighter on the application front ends.

#### Can posts once created be edited?

Yes! They can be. Typos, grammatical errors are a part of life. A post once created can be updated. Just use the [`sdk.post.update`](https://github.com/gumhq/sdk/blob/master/packages/gpl-core/src/post.ts#L63) method from our SDK.

#### How does Gum use account compression?

\
Gum leverages [Solana's account compression](https://github.com/solana-labs/solana-program-library/tree/master/account-compression) heavily to save on rent to a point where it can be subsidised by applications.&#x20;

1. We [initialize](https://github.com/gumhq/gpl/blob/master/programs/gpl\_compression/src/instructions/tree\_config.rs#L32) a concurrent merkle tree per profile.
2. All high velocity actions like posts, follows and reactions are[ hashed into a merkle tree](https://github.com/gumhq/gpl/blob/master/programs/gpl\_compression/src/instructions/post.rs#L88-L106).&#x20;
3. Once the transaction is executed our indexer fetches the instruction data and transaction log so that applications can use them.
4. Applications can always verify the authenticity of the data from the indexer by just [verifying the proof from the indexer against the merkle tree on chain. ](https://github.com/gumhq/gpl/blob/master/programs/gpl\_compression/src/utils.rs#L101)\
   \
