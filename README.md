# Agentic Fediverse

The "Agentic Fediverse" is the idea of a new kind of [network federation](https://en.wikipedia.org/wiki/Federation_(information_technology)), a complementary iteration on the concept of federation currently established with [ActivityPub]. It's an experiment and something that we at the [Commune](https://github.com/commune-os) group are
still trying to define more concretely.

> **Note:** This is a temporary place for collecting links about the agentic fediverse, until this can be hosted directly on [Weird.one](https://weird.one).

[ActivityPub]: https://activitypub.rocks/

### Tenets

We are still working on defining the core tenets of the agentic fediverse, but here is what we have so far.

An agentic fediverse is..

1. **A fediverse of agents**; agent-centric, as opposed to server-centric.
2. **Local-first**; solve local problems for local people.
3. **Accessible by default**; inaccessibility is a bug.
4. **Systematically consensual**; architected on the basis of informed consent.

### Primer

The word *agentic* occurs primarily in the social sciences, pertaining to an individual's agency, often used interchangeably with _autonomy_ or _self-determination_.

> In [social science](https://en.wikipedia.org/wiki/Agency_(sociology)), agency is the capacity of individuals to have the power and resources to fulfill their potential.

To be _agentic_ is to be agency-driven and agent-centric. An agentic system optimizes for agency, for example by measuring _user agency_ as a metric of success.

# Definitive Topics

## The minimal definition of user agency

Gordon Brander, jamming on recurring ideas in the web3 space, proposes a _[minimal definition of user agency](https://newsletter.squishy.computer/p/the-minimal-definition-of-user-agency):_

- Own your ID
- Own your content
- Own your contacts

> Why do you need all three?
> 
> If you don’t own your content, you’re stuck. Owning your content is necessary for agency! However, it is not sufficient, because…
> 
> If you own your content, but don’t own your contacts, then you will lose your entire social graph when you switch services. Network effect will keep you locked in.
> 
> If you own your content and contacts, but don’t own your ID, you don’t really own anything. Why? IDs are upstream of access. Specifically, this is about who owns your name, and the cryptographic keys that secure your ID, under the hood. If your name is owned by an authority, you lose it if you leave. If your keys are owned by an authority, they are in control. They can lock you out of your stuff, snoop on your private messages, delete your account, or refuse to let you move it elsewhere. So self-sovereign IDs and keys are crucial for agency.
>  
> When you own your ID, content, and contacts, you have agency, because you have credible exit. You can seamlessly change services and bring everything important with you, like switching carriers for your mobile phone.

## Credible exit

[Vendor lock-in](https://en.wikipedia.org/wiki/Vendor_lock-in), e.g. by way of data lock-in, is by definition an anti-agentic feature. European internet users' _right to exit_ is even codified by law in the [GDPR's Right of Access](https://www.dataprotection.ie/en/individuals/know-your-rights/right-access-information).

From Gordon Brander's [Credible exit](https://newsletter.squishy.computer/p/credible-exit):

> #### Dimensions of credible exit
> 
> I think there may be multiple dimensions along which to think about credible exit. An app might provide some or all of them.
> 
> **I can export my data:** This is pretty bog-standard due to GDPR regulations now. Often what you get is a zip full of JSON files. Pretty lackluster. JSON isn’t something that makes sense outside of an app.
> 
> Export also has the downside of being static. If you continue to use the app, your export becomes invalid. This makes export only really useful for hard exit. Still, it’s better than nothing. I might say “export considered harm reduction”.
> 
> **I can sync my data**, or at least export and re-import it multiple times. This resolves the “dead export” problem. It also makes export useful for other use-cases, like backup and basic interoperability between tools. One way to accomplish this is with immutable data. If the file never changes, it’s easy to sync. iTunes music library is a good example. CRDTs are a promising primitive for data that is frequently updated.
> 
> **My data is in a useful format:** exit happens through a common formats that work in other apps. Plain text, CSV, PNG, PDF, SVG, MP3 are all good examples of useful formats. Camera Roll is a standout example here. You can drag and drop images, in and out of the app freely.
> 
> **My data lives in local files.** iTunes is a standout example. You can always take your MP3 files with you. Files are great because you have the bytes! Files also enable credible exit to emerge retroactively! New apps can come along and implement the file formats of other popular apps, uplifting their file format into a de facto protocol.
> 
> **Multiple apps can share the same data over a permissionless API.** This is where credible exit transcends itself and becomes broad interoperability.
> 
> **The app is open source.** A protocol will never capture the full fidelity of the app experience, but if the protocol and app are both open, you might unlock much deeper credible exit. Mastodon is a great example here.

Continued in [Freedom to exit](https://newsletter.squishy.computer/p/freedom-to-exit):

>> **Freedom to exit:** you can take your data and leave, and no one can tell you no.
> 
> The web does not support the freedom to exit. The server owns the data, so the server can tell you no. The same is true of most mobile apps. Some save files to folders, but most trap data in the app. The app can tell you no. This lock-in is used as a competitive moat for aggregators, and a business model for software-as-a-service.
>
> So, how do we fix this? As far as I can tell there are just three ways to guarantee freedom to exit: decentralized protocols, local-first data, or legal agreements.
> 
> - **Decentralized protocols:** no one can tell you no because the data is distributed across multiple peers controlled by different parties. If one peer tells you no, you just ask another.
> - **Local-first data:** no one can tell you no because you already have your data.
> - **Legal agreements:** no one can tell you no because you have a contract or license in place that guarantees access to your data.

## The web is for user agency

https://berjon.com/user-agency/

> Technologists trying to maximise user agency often fall into the trap of measuring agency by looking only at time saved (in the same way that they fail to understand what people want to do when they measure time spent). On the surface, the idea seems straightforward: spend less time on one thing, have more time for other things! That would seem to fit our mandate of improving "What each person is able to do and to be". And all other things being equal that can be true, but the devil is in the details: the enjoyment of doing the thing, the value in knowing how to do it, or the authority over outcomes. Even things that many would consider chores aren't always best automated or delegated away: you may not wish to clean your house but you might want a say in the chemicals introduced into your home, about how your things are organised, or over whether your house can be mapped by a robot and data derived from that map sold to the highest bidder. Not all leisure is liberation.
> 
> The more detail we have on a piece of technology that may be part of the Web, the more readily we can assess it in very specific ways that capture aspects of improved user agency. In fact, that's something that the Web community has been doing for a long time. Consider:
> 
> - The great level of detail that has gone (and continues to go) into specifying [how to make the Web and Web content accessible](https://www.w3.org/WAI/standards-guidelines/). These guidelines and techniques can, in exceedingly concrete ways, push for a world in which disability does not limit agency.
> - An equally-impressive trove of actionable principles can be found in the [Internationalization work](https://www.w3.org/blog/international/). This empowers people to use the Web in the languages of their choice. We will never celebrate the work of the [Unicode Consortium](https://home.unicode.org/) enough. Bringing all of the world's languages into a unified system of character encoding is a historical achievement that "[respects and empowers users](https://home.unicode.org/about-unicode/)".
> - It's hard to act freely if you can't act safely, which makes [work on security](https://www.w3.org/Security/) core to the agency project. [RFC8890](https://www.rfc-editor.org/rfc/rfc8890) ("The Internet is for End Users") captures this well when it states that "User agents act as intermediaries between a service and the end user; rather than downloading an executable program from a service that has arbitrary access into the users' system, the user agent only allows limited access to display content and run code in a sandboxed environment. End users are diverse and the ability of a few user agents to represent individual interests properly is imperfect, but this arrangement is an improvement over the alternative — the need to trust a website completely with all information on your system to browse it." This trust is empowering.
> - And the same can be said about [privacy](https://www.w3.org/Privacy/), which is key to trust as well. Privacy further matters (as discussed in the [Privacy Principles](https://w3ctag.github.io/privacy-principles/)) in that it includes the right to decide what identity you present to others in which contexts. Additionally, widespread data collection creates information asymmetries and information asymmetries create power asymmetries. The issue here isn't so much that data might be used to support mind-controlling AI snake oil but rather that it powers more mundane (and far more effective) manipulation techniques such as [hypernudging](https://www.tandfonline.com/doi/abs/10.1080/1369118X.2016.1186713).
> 
> These shared foundations for Web technologies (which the W3C refers to as "horizontal review" but they have broader applicability in the Web community beyond standards) are all specific, concrete implementations of the Web's goal of developing user agency — they are about capabilities. We don't habitually think of them as ethical or political goals, but they are: they aren't random things that someone did for fun — they serve a purpose. And they work because they implement ethics that get dirty with the tangible details.

## The Great Untangling (series)

### Reclaiming (my) digital identity

https://blog.erlend.sh/reclaiming-my-digital-identity

> #### Connective tissue
> 
> I've grown up in an unprecedented time of connective magic. Unlike my ancestors, I have known not tens, not hundreds, but thousands of people with whom I co-created something of value. Such is the power of the digital age. My deepest connections exist locally, offline. Yet many of those connections were initially mediated through the incredible connective tissue of the internet.
> 
> My life as a somewhat odd person would have been a profoundly lonely one had it not been for the online spaces where I found The Others; fellow weirdos interested in play-crafts like storytelling and game development, coupled with an obsession for openness as a means to digital emancipation. To most of the thousands of people I've come across in my two decades as a netizen, the digital expression of my persona is my entire persona. I hope to live long enough to engage in a physical handshake or even an embrace with many of my online friends, but I recognize that a large number of these connections will forever remain purely digital.
> 
> Therefore it is acutely important to me that my digital identity properly reflects my truest self. I want the people in my life to have seen and known the real me, regardless of whether they came into contact with my physical or digital being.
>   
> #### Identity prison
> 
> And therein lies my predicament: Ever since I first logged on to the internet, I've never had legitimate ownership of my own digital identity. My digital expression has always been mediated through some higher power. Sadly not of the paternal kind that intends to lift my spirit up until I can stand on my own.

### Websites as the atomic matter of the internet
> https://blog.erlend.sh/weird-web-pages#website
> I consider the personal website to be the smallest possible building block of web identity. Once you wanna go past the observer (READ) level to the contributor (WRITE) level as a netizen, you’re gonna need a material web-persona to make yourself known. Unfortunately we never made personal websites easy enough to build, so the likes of Facebook became mainstream persona providers.
>
> (...)
> 
> ### Web pages materialize the internet
> 
> The size of the internet can be measured in the atomic mass of the websites it's made up of. We collectively materialize the internet with every additional web page we create.

> https://blog.erlend.sh/assembling-community-os
> Digital autonomy begets individual freedom begets fairness & equality.
> 
> The hopeful possibility of this moment lies in the open-social web protocols which make up the foundations of a comms & coordination ecosystem owned and operated by the general public.

## Decentralize ownership; Recentralize agency

> https://blog.erlend.sh/weird-netizens
> To free ourselves of our current predicament, we must simultaneously de-centralize and re-centralize identity.
> 
> By de-centralizing the ownership of identity away from platform monopolies and back to individuals, we can re-centralize the agency of personhood.
> 
> Once more for clarity:
> Decentralize ownership.
> Recentralize agency.
> 
> The central authority of ones digital identity must first and foremost be the individual themselves. That's how we regain our digital sovereignty.
>
> (...)
>
> ### Decoupling Identity
> 
> All mainstream identity providers get you hooked into their ID-network by means of a tight coupling between a light identity layer plus a heavy service:
>
> GitHub: ID + git
> Discord: ID + chat
> Gmail: ID + email
> The indivisibility of this coupling weakens our digital sovereignty. Even if I stopped using Gmail for email, I still rely heavily on it for my authentication to hundreds of sites & services. It’s part of their lock-in scheme.
> 
> Gmail et.al. make identity confusing because they've made it appear necessarily coupled with an overarching complexity like email or a social network. But identity should stand on its own. In fact it is paramount that our identity is not owned by a personal-data-loving megacorp because there's nothing more valuable for them to keep locked up than the very essence of your digital self.

# Resources

- https://newsletter.squishy.computer/p/credible-exit
- https://newsletter.squishy.computer/p/redecentralization
- https://newsletter.squishy.computer/p/the-minimal-definition-of-user-agency
- https://newsletter.squishy.computer/p/freedom-to-exit
- https://newsletter.squishy.computer/p/decentralizability
- https://berjon.com/user-agency/
- https://blog.erlend.sh/reclaiming-my-digital-identity (The Great Untangling series)
- https://socialhub.activitypub.rocks/t/autonomous-identity-for-the-pluriverse-based-on-oauth-oidc/3675
- https://blog.erlend.sh/weird-netizens
- [Fedi v2](https://www.thepaperpilot.org/garden/fedi-v2/) by `@thepaperpilot`
- [How to Federate?](https://zicklag.katharos.group/blog/how-to-federate/) by [@zicklag](https://github.com/zicklag)

## Related Projects:

- https://blog.commune.sh/weird-happenings/
