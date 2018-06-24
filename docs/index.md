## What is the UAA?


## Why *not* use the UAA?

Whilst the UAA is a solution to several critical problems it is not the only solution. If your web applications or mobile applications have user accounts and login pages then you have already solved some of the user authentication and authorization problems already.

If you've delegated some then you might have delegated them to GitHub, Google Accounts, Microsoft Active Directory, Facebook, Twitter, or some other pre-existing system that will tell your site that a user is a specific user.

As an example, I want to use your application and you allow me to authenticate to your application via GitHub. That is, you ask GitHub to prove who I am and allow GitHub to tell you my personal information.

On your site I click your "Login via GitHub" button.

I would be redirected to https://github.com/ to first login if I were not already signed in. As Dr Nic Williams, I would login to GitHub as [@drnic](https://github.com/drnic). Since only I know my GitHub password, GitHub trusts that the `@drnic` account is me. Except GitHub doesn't truly trust me, so it asks me for a second form of proof. I take out my phone, open the Authy application (or another like Google Authenitcator) and copy in a 6 digit number. Finally GitHub believes that `@drnic` is me using two factors of authentication (2FA) - my password and my code from my phone.

Next, GitHub asks me if I grant your application permission - authorization - to access my GitHub account. Your application only wants my personal profile information - my name and email - and so that is all I am asked to authorize. I click "Authorize".

GitHub now redirects me back to your web application or mobile application. Your application is given my personal information - my email and name - and your application respects GitHub's decision and the information it receives from GitHub. If GitHub believes that I am `@drnic` and my name is "Dr Nic Williams", then your application believes it too. Your application never has to ask me for my name nor email.

Your example application also supports organizations - as a user I can see and edit content only within teams/organizations that I belong.

Instead of you development team spending a lot of time implementing its own UI and business logic for orgnaizations - how to invite and revoke people from teams - you defer again to GitHub. When GitHub redirected me back to your application, it also told you which GitHub organizations I am a member of, and which teams of each organization. For example, I am a member of the [@starkandwayne](https://github.com/starkandwayne) - an excellent consultancy for Cloud Foundry, Kubernetes, and enterprise cloud systems. Your application also has a `@starkandwayne` organization so I am automatically granted permission (authorization) to read and modify its contents within your application.

If not Github, your application might use Google Accounts (`drnic@starkandwayne.com` belongs to a `@starkandwayne.com` Google Account) or an in-house Microsoft Active Directory organization.

In all these variations your application does not have to touch or store or rotate passwords or two-factor authentication systems. It does not have to implement UIs for the management of membership of organizations/teams. And anything you didn't have to implement is something you don't have to continually maintain over the next decade. You've kept your application simpler, and thus its behavior more well known to more people.

## Why do I write this book?

The UAA was first written to be a component of Cloud Foundry for its 2011 public open source release. I've watched or been heavily involved with Cloud Foundry since that time, over 7 years ago from 2018 at the time of writing, and yet I never truly understood the UAA. It just worked. I thought, what was to truly know?

If I logged into Cloud Foundry with its CLI - `cf login` or `cf auth` - then I knew the UAA was involved. If I logged into [Pivotal Web Service](https://run.pivotal.io), [GE Predix](https://http://predix.io/), [Swisscom Cloud](http://cloud.swisscom.com/), or any other public or private Cloud Foundry, then I was visibly redirected to the UAA (often it has the subdomain `login`, for example https://login.run.pivotal.io/) prior to be returned to a Cloud Foundry web console. A few years ago the UAA popped up inside of BOSH and then CredHub, but I was never troubled by more than the extra RAM requirements of running the additional Java/Tomcat process that is the UAA.

I knew the ideas behind "Single Sign On" (SSO), OAuth2, and OpenID; but I had never been required to get my hands dirty and write a client application from scratch that integrated with the UAA. I had never had to integrate the UAA with backed user address books such as Microsoft Active Directory. I had never had to setup users and permissions with the UAA. Someone else always seemed to get around to tasks relating to the UAA before I did.

Many aspects of the UAA were still unknown to me.

The UAA was written in Java and used the web framework Spring; the former I disliked, and the latter was a huge learning curve for the sole purpose of being able to read the UAA source code.

The UAA web interface was also never aesthetically pleasing; was not visited for very long by users, nor did users ever choose to visit the UAA: you would try to visit a website, your user session would have expired and were redirected to the UAA against your will, you entered your username and password, and were then quickly returned to the actual website you were trying to use.

The UAA web interface does not perform any administration or configuration functions. It only allows people to sign in, to setup and use multi-factor authentication (also known as MFA, two-factor authentication, or 2FA), to confirm that a third-party application is allowed to access their UAA personal information, or to revoke that permission at a later time.

Instead, an administrator must interact with the UAA API or use a primitive CLI to add or modify users and third-party client applications. APIs and low-level CLIs are fantastic for power users, but they not very welcoming to myself: a brand-new user for seven years in a row.

The UAA could be the user authentication (who am I?) and authorization (what am I allowed to do?) backend for every web application or API, but I don't feel like the UAA team, its sponsors (primarily Pivotal), nor the Cloud Foundry Foundation do the UAA justice and promote it for such a broad mission. It has no dedicated marketing site to learn more, nor a simple Docker image to get started.

But I could see the wide reaching potential for the UAA beyond Cloud Foundry; as I'm sure many people do who've explicitly noticed the UAA. It is incredibly powerful, it has a well documented API, it is open source, it is written in a popular web framework (Spring), and it is free. It could be a huge boon to software developers and systems administrators around the world.

So I sat down to explain the UAA to myself with a sequence of tutorials that I wished were written for me, and I hope are now useful to you and your work friends.

Along the way I wrote the `uaa-deployment` CLI to make it much easier to deploy the UAA locally and to any cloud (Amazon Web Services, Google Compute, Microsoft Azure, VMWare vSphere, OpenStack, and more).

I hope you discover the incredible power of the UAA and learn to feel empowered to delegate to it all your user authentication and authorization needs. It will do so much for you that you yuorself will no longer have to implement, or apologise for not having implemented yet.