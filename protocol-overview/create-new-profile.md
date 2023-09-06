# 👤 Create new Profile

```typescript
import { useCreateProfile, useGumContext, useUploaderContext } from '@gumhq/react-sdk';
import { useWallet } from '@solana/wallet-adapter-react';
import { useState } from 'react';
import styles from '@/styles/CreateProfile.module.css';
import Header from '@/components/Header';

export function ProfileCreation() {
  const [profileName, setProfileName] = useState('');
  const [profileBio, setProfileBio] = useState('');
  const [profileUsername, setProfileUsername] = useState(''); // username is the screen name
  const [profileAvatar, setProfileAvatar] = useState('');
  
  const wallet = useWallet();
  const { publicKey } = wallet;
  const { sdk } = useGumContext();
  const { createProfileWithDomain, createProfileError } = useCreateProfile(sdk);
  const { handleUpload, uploading, error } = useUploaderContext();

  const createProfile = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    if (!publicKey) {
      console.log("No public key found");
      return;
    }

    let profilePDA = await sdk.profile.getProfileByDomainName(profileUsername);
    if (profilePDA) {
      console.log("Profile account found with username", profileUsername);
      return;
    }

    const profileMetadata = {
      name: profileName,
      bio: profileBio,
      avatar: profileAvatar,
    };

    const uploadResponse = await handleUpload(profileMetadata, wallet);
    if (!uploadResponse) {
      console.error("Error uploading profile metadata");
      return false;
    }

    const profileResponse = await createProfileWithDomain(uploadResponse.url, profileUsername, publicKey);
    if (!profileResponse) {
      console.error("Error creating profile");
      return false;
    }

    console.log("Profile created successfully", profileResponse);
  };

  return (
    <>
      <Header />
      <div className={styles.container}>
        <h1 className={styles.title}>Create Profile</h1>
        <form onSubmit={createProfile}>
          <label htmlFor="name" className={styles.label}>Name</label>
          <input type="text" id="name" value={profileName} onChange={(event) => setProfileName(event.target.value)} className={styles.input} />
          <label htmlFor="bio" className={styles.label}>Bio</label>
          <input type="text" id="bio" value={profileBio} onChange={(event) => setProfileBio(event.target.value)} className={styles.input} />
          <label htmlFor="username" className={styles.label}>Username</label>
          <input type="text" id="username" value={profileUsername} onChange={(event) => setProfileUsername(event.target.value)} className={styles.input} />
          <label htmlFor="avatar" className={styles.label}>Avatar</label>
          <input type="text" id="avatar" value={profileAvatar} onChange={(event) => setProfileAvatar(event.target.value)} className={styles.input} />
          <button type="submit" className={styles.submitButton}>Create Profile</button>
        </form>
      </div>
    </>
  );
}

export default ProfileCreation;
```
