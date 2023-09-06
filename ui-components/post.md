---
description: >-
  This component allows you to render a post. It takes in the post and profile
  data as props.
---

# ✍ Post

```tsx
import { Post, PostMetadata, ProfileMetadata } from '@gumhq/ui-components';
// import { Post, PostMetadata, ProfileMetadata } from '@gumhq/react-native-components';
// Use the above import statement for React Native

function Main() {
  const profile: ProfileMetadata = {...}
  const post: PostMetadata = {
    type: "text",
    content: {
      format: "markdown",
      content: "Hello World"
    }
  }
  return (
    <Post
      profileData={profile}
      data={post}
    />
  );
}
```

<figure><img src="../.gitbook/assets/Screenshot 2023-03-03 at 4.50.10 PM.png" alt=""><figcaption><p>An example of a Post rendered using this library</p></figcaption></figure>
