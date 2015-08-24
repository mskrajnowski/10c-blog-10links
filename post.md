# Case study: 10Links

## The idea

At 10Clouds we produce a lot of links to interesting articles, github repositories and applications on a daily basis. They land on our mailing list and various Slack channels, and after a while they get buried underneath other conversations.

For our [5th Hackday event][5th hackday post], we decided to tackle the problem of the disappearing knowledge base by creating a simple app gathering links, so we can easily access them in the future.

## Targets

![Logos](tech-cloud.png)

The theme of the latest hackday was trying out various backend as a service products. We've decided to take it to the extreme and set ourselves the goal of not using any custom servers, focusing on using 3rd party products and minimal glue code.

Before the event we've researched available tools and selected:
- [Parse][] as our data store
- [Auth0][] as the authentication provider
- [Readability API][] for article extraction
- [Zapier][] for link extraction

Finally, the web app was built using the [Aurelia framework][aurelia] and [Materialize CSS][], and hosted on [GitHub Pages][].

## Gluing it all together

First off, the user has to sign in using our Google domain and obtain a Parse session token in order to work with our backend. Since Parse doesn't provide a simple way to log in Google users, we've added Auth0 to the stack. 

Auth0 provided both a widget for logging users in - [Auth0 Lock][], as well as the ability to write [custom rules][auth0 rules] in javascript, which let us obtain a Parse session token as a part of the authentication flow.

![Authentication flow](auth-flow.png)

Once logged in, the user can view the latest links gathered by our backend services. The whole link extraction flow was built around Zapier, as it enabled us to easily create hooks that pulled data from our Slack and Google group and sent it to Parse. On the Parse side, we've glued everything together usign [Parse Cloud Code][], similarly to Auth0 rules, it enabled us to write custom code, which was responsible for both extracting links from messages, as well as automatically extracting articles from these links using the [Readability Parser API][readability parser].

![Pull link flow](pull-flow.png)

## 10Links

![End result](result.png)

During the event, we've managed to implement only the core of 10Links, that is fetching articles from our Slack channels and the mailing list, displaying them in a simple grid view as well as an article view with the markup extracted by Readability. We've also added a simple up/down vote for each article.

However, we treat this as the start for building a very useful application with features like tagging, full-text search and article suggestions, personalized reading list, etc. We're also looking forward to open-sourcing the code base as soon as possible.

## Final thoughts

We've proven to ourselves that creating an application based purely on 3rd party backend services is possible, however, there are a few gotchas. 

Of course there's the issue of costs, each of the building blocks costs money to run in a production environment and it might very well be less expensive to simply roll out your own backend for the given features. In the case of 10Links we will probably replace the authentication flow provided by Auth0 with [a custom one based on Parse](https://parse.com/tutorials/adding-third-party-authentication-to-your-web-app), purely for that reason.

Another issue is managing the glue code, custom code snippets running "in the cloud", such as Auth0 rules and Parse cloud code. As a developer you want to have all your code well organised in a VCS. It's not a great developer experience to push the code to the repository and then have to manually copy&paste a block of code to a given service. Parse makes this a bit better by providing a [CLI][parse cli] for deploying updates, so you could create a git hook, which would deploy the code automatically.

[5th hackday post]: http://10clouds.com/blog/hackday-v-why-we-all-love-codefesting/
[aurelia]: http://aurelia.io/
[auth0]: https://auth0.com/
[auth0 lock]: https://auth0.com/lock
[auth0 rules]: https://auth0.com/docs/rules
[google groups]: https://groups.google.com
[materialize css]: http://materializecss.com/
[parse]: https://parse.com/
[parse cli]: https://parse.com/docs/js/guide#command-line
[parse cloud code]: https://parse.com/docs/js/guide#cloud-code
[readability api]: https://www.readability.com/developers/api/
[readability parser]: https://www.readability.com/developers/api/parser
[readability]: https://www.readability.com/
[slack]: https://slack.com/
[zapier]: https://zapier.com
[github pages]: https://pages.github.com/
