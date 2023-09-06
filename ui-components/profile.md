---
description: >-
  This component allows you to render a profile. It takes in the following data
  structure as props.
---

# 🖼 Profile

{% code lineNumbers="true" %}
```tsx
import { Profile, ProfileMetadata } from '@gumhq/ui-components';
// import { Profile, ProfileMetadata } from '@gumhq/react-native-components';
// Use the above import statement for React Native

function Main() {
  const profile: ProfileMetadata = {
    name: "Kunal Bagaria",
    bio: "Software Engineer @ Gum",
    username: "kunal",
    following: 5,
    followers: 500,
    avatar: "https://i.imgur.com/uGv5Zca.jpg",
  }
  return (
    <Profile data={profile} />
  );
}
```
{% endcode %}

<figure><img src="../.gitbook/assets/Screenshot 2023-03-03 at 4.50.04 PM.png" alt=""><figcaption><p>An example of a profile rendererd using this library</p></figcaption></figure>
