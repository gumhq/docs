# 💁♂ Introduction

<figure><img src="https://arweave.net/jvCDqOXmjvmBntJW2n_VqrWVfyAS8_UxCpdpCj57Hfw" alt=""><figcaption></figcaption></figure>

Web3 and consumer crypto apps have reached a point where there’s a vast ecosystem of projects being built for different problems. However, there is one thing common among all of them. They are all trying to figure out a way to reduce the UX pain that comes with wallet interactions.

If we’re going to onboard the masses onto crypto, we need to improve and strive for a UX benchmark that existing web2 applications have already set for users. Part of the reason why this goal has been hard to achieve is that interacting with wallets and signing transactions forms a major part of a user’s journey while using any crypto app. And that’s not fun at all.

#### The DApp UX Trilemma

Before we dive in, we need to understand the current problem statement we face when it comes to using a dApp. The best way to describe this is through what we call the DApp UX Trilemma.

<figure><img src="https://arweave.net/M8u4_NobQwDB4k7jbhAfL9i2TzJ0D6vhvHWgL7U8_cA" alt="The DApp UX Trilemma"><figcaption><p>The DApp UX Trilemma</p></figcaption></figure>

The DApp UX Trilemma refers to the tradeoff between three critical features that are essential to a dApp’s user experience. These features are:

* User Experience (UX): A smooth user experience should not involve multiple wallet popups for signatures at every step of the way. This breaks a user’s flow and reduces stickiness considerably. The UX should ideally be at par, if not better than web2 applications out there.
* Security: A secure wallet should be able to protect the user's private keys from unauthorized access or theft.
* Interoperability: An interoperable wallet (like Phantom, Backpack etc.) ideally allows users to use the same wallet across multiple applications. They allow the identity linked to the wallet to be composable across platforms and not be locked into a particular app.

Now, let’s look at some current solutions that take care of two out of the three here:

* Hardware/Native wallets: Cold wallets like a Ledger Nano S provide the ultimate security against hacks and can also be used to connect with dApps via a browser wallet. Hence, they provide both security and interoperability but the UX of using and connecting a hardware wallet multiple times is not ideal at all.
* In-app wallets: We’ve seen a few emerging in-app wallets recently which provide self-custody along with a great user experience because they assume the role of an isolated wallet and approve all the transactions they do automatically. But, this comes with a cost. Being in-app wallets, you’re really trusting the app that gives you the wallet and promises not to rug you and keep your assets secure. Additionally, these wallets also lock a user to the app itself. They fracture the identity and reputation that is linked to a wallet address and take away the composability of identity across other apps that web3 uniquely enables.
* Browser wallets (with auto-approve features): Browser wallets that have auto-approve functionality for certain transactions provide interoperability and a much better UX but pose a major security threat and can become vulnerable to hacks.

This might seem like a smaller problem, but it will eventually play a huge role in whether we achieve mass adoption or not.

This friction we currently workaround will not be received well by users who’re accustomed to seamless social apps and games in the web2 world. For a social app or game to hook a new user, it needs to provide value and a reason to stay as well as come back pretty much instantly. Achieving this with a crypto app that requires multiple transaction signatures at every step is a tall order.

This is why we’re launching Session Keys.

#### What are Session Keys?

Session Keys are ephemeral keys with fine-grained program/instruction scoping for tiered access in your Solana Programs.

They allow users to interact with an app under particular parameters like duration, the maximum amount of tokens spent, amount of posts users can make or any other function specific to an app’s use case.

Apps can also have a layered security model which allows tiered access to a session key making sure a user’s funds and assets are always secure and can’t be accessed by the session keys.

This type of layered security is a standard model in web2 applications as well with measures such as firewalls, user authentication, 2FA etc. to protect against threats like SQL injection, cross-site scripting, and data breaches. This approach provides a stronger defence against attacks and helps ensure the security and integrity of apps. This is now possible in web3 with the use of Session Keys at the contract level.

Session Keys solve all 3 problems in the DApp UX Trilemma:

* Security: Session Keys are fully scoped and smart contracts can choose the instructions that can accept a particular session key. The expiry and access are stored at the contract level which immunes session keys from potential security vulnerabilities. As spoken above, they also offer layered security safeguarding a user’s assets even more.
* User Experience (UX): Session keys are a giant leap for improving UX in crypto as they take away the need for repeated wallet popups while a user performs certain actions in a dApp for a particular duration.
* Interoperability: Session keys work at the contract level and hence, users can use their usual browser wallets while interacting with a dApp that provides them with a session key in the background. This preserves the interoperability of apps and smoothes the UX even further.

Let’s take an example of how a session key can change a user’s journey to make a post in a decentralised social app:

<figure><img src="https://arweave.net/da0aYEyvkVCUtQzE9lGaYUviH7kUU7rVtvW4MvDvIN0" alt="Difference in UX with and without Session Keys"><figcaption><p>Difference in UX with and without Session Keys</p></figcaption></figure>

#### What are some use cases?

There are so many use cases that Session Keys unlock. Developers can now create web2 standard user flows while also being able to leverage crypto and the native use cases that it brings with it.

Here’s a non-exhaustive list of use cases:

* Smooth and sticky social media apps that don’t suck
* Forums and forms without repeated popups and confirmations
* A seamless experience for games that have in-app NFTs
* An uninterrupted gaming experience
* A layered security model for all crypto apps

##

###

###
