# 📄 Create new Post

Before creating a post, you will need to create a `metadataUri` that contains the metadata for your post in JSON format. We suggest using a decentralised storage solution such as Arweave, IPFS, etc. to create this `metadataUri`.

Here is an example of how you can create a new post:

```typescript
import { getProfileAccount } from "@/utils";
import { useCreatePost, useGumContext, useSessionWallet, useUploaderContext } from "@gumhq/react-sdk";
import { GPLCORE_PROGRAMS } from "@gumhq/sdk";
import { useWallet } from "@solana/wallet-adapter-react";
import { PublicKey } from "@solana/web3.js";
import { useEffect, useState } from "react";
import styles from '@/styles/Home.module.css';
import PostDisplay from '@/components/PostDisplay';

export type PostData = {
  content: {
    content: string;
    format: string;
  };
  type: string;
  authorship: {
    signature: string;
    publicKey: string;
  };
  app_id: string;
  metadataUri: string;
  transactionUrl: string;
};

const PostCreator = () => {
  const [postContent, setPostContent] = useState("");
  const { sdk } = useGumContext();
  const wallet = useWallet();
  const session = useSessionWallet();
  const { publicKey: sessionPublicKey, sessionToken, createSession, sendTransaction }  = session;
  const { handleUpload, uploading, error } = useUploaderContext();
  const { createUsingSession, createPostError } = useCreatePost(sdk);
  const [profile, setProfile] = useState<PublicKey | undefined>(undefined);
  const [posts, setPosts] = useState<PostData[]>([]);

  console.log(`Error: ${createPostError}`);
  
  useEffect(() => {
    const initializeProfile = async () => {
      if (wallet.publicKey) {
        const profileAccount = await getProfileAccount(sdk, wallet.publicKey);
        if (profileAccount) {
          setProfile(profileAccount);
        } else {
          console.log("Profile account not found, please create profile");
        }
      }
    };
    initializeProfile();
  }, [sdk, wallet.publicKey]);
  
  const refreshSession = async () => {
    if (!sessionToken) {
      const targetProgramId = GPLCORE_PROGRAMS["devnet"];
      const topUp = true; 
      const sessionDuration = 60;
      return await createSession(targetProgramId, topUp, sessionDuration);
    }
    return session;
  };

  const handlePostCreation = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const updatedSession = await refreshSession();

    if (!updatedSession || !updatedSession.sessionToken || !updatedSession.publicKey || !updatedSession.signMessage || !updatedSession.sendTransaction || !profile) {
      console.log(` profile: ${profile}`);
      console.log("Session or profile details missing");
      return;
    }

    const postArray = new TextEncoder().encode(postContent);
    const signature = await updatedSession.signMessage(postArray);
    const signatureString = JSON.stringify(signature.toString());

    const metadata = {
      content: {
        content: postContent,
        format: "markdown",
      },
      type: "text",
      authorship: {
        publicKey: updatedSession.publicKey.toBase58(),
        signature: signatureString,
      },
      app_id: "gum-quickstart",
      metadataUri: '',
      transactionUrl: '',
    };

    const uploader = await handleUpload(metadata, updatedSession);
    if (!uploader) {
      console.log("Error uploading post");
      return;
    }

    const postResponse = await createUsingSession(uploader.url, profile, updatedSession.publicKey, new PublicKey(updatedSession.sessionToken), updatedSession.sendTransaction);
    if (!postResponse) {
      console.log("Error creating post");
      return;
    }

    metadata.metadataUri = uploader.url;
    metadata.transactionUrl = `https://solana.fm/tx/${postResponse}?cluster=devnet-solana`;

    setPosts((prevState) => [metadata, ...prevState]);

    setPostContent("");
  };

  return (
    <>
    <main className={styles.main}>
      <div className={styles.container}>
        <form onSubmit={handlePostCreation}>
          <input
            type="text"
            value={postContent}
            onChange={(e) => setPostContent(e.target.value)}
            placeholder="What's on your mind?"
          />
          <button type="submit">Submit</button>
        </form>
      </div>
      <PostDisplay posts={posts} />
    </main>
    </>
  );
};

export default PostCreator;
```

The `metadataUri` should contain a JSON object with several required fields:

1. `content`: This field holds the content of the post. The content can be in different forms, such as `Blocks`, `Image`, `Video`, `Json`, or `Text`.
2. `type`: This is to specify the type of content in the `content` field. It can be any of the following strings: `"blocks"`, `"image"`, `"video"`, `"json"`, or `"text"`.
3. `app_id`: This field is a unique identifier used to associate a post with a specific app. It helps in categorising posts based on the applications they were created in. For instance, if a post was created in an application called "Gum Quickstart", the `app_id` could be `"gum-quickstart"`.
4. `authorship`: This is an object that contains the signature of the content created by the author and the author's public key. The signature serves as an authentication measure, ensuring that the content is indeed created by the author, and the public key is used to identify the author of the content.

Here's how these fields could be structured in a `metadataUri` JSON object:

```json
{
  "content": "<content goes here>",
  "type": "<content type goes here>",
  "app_id": "<unique app identifier goes here>",
  "authorship": {
    "signature": "<content signature by the author>",
    "publicKey": "<public key of the author>"
  }
}
```

You can check out the preferred data type for the content of the metadataUri [here](https://github.com/gumhq/sdk/blob/master/packages/gpl-core/src/postmetadata.ts#L45-L57).
