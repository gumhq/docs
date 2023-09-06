---
description: >-
  This component allows you to render a feed of posts. It takes in an array of
  objects which includes the post and profile data.
---

# 🤩 Feed

```tsx
import { Feed, PostMetadata, ProfileMetadata } from '@gumhq/ui-components';
// Feed is currently not available for React Native

function Main() {
  const profileData: ProfileMetadata = {...};
  const postData: PostMetadata = {...};
  const feed = {
    { post: postData, profile: profileData },
    { post: postData, profile: profileData },
    { post: postData, profile: profileData },
  };
  return (
    <Feed posts={feed} skip={0} show={feed.length} gap={0.5} />
  );
}
```
