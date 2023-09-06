---
description: We have a few Material-UI icon buttons that you can use in your application.
---

# 😄 Icons/Reaction Buttons

```tsx
import { ReplyButton, LikeButton } from '@gumhq/ui-components';

function Main() {
  const likeData = {
    liked: false,
    like: async () => console.log('Like'),
    unlike: async () => console.log('Unlike'),
  };
  return (
    <div>
      <ReplyButton onClick={() => console.log('Handle Reply Button Click')} />
      <LikeButton data={likeData} />
    </div>
  );
}
```
