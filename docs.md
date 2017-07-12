# Launchpad Documentation

All of your questions about Launchpad, answered.

## Ownership and visibility

Every pad on Launchpad is owned by the user that created it. The code and endpoint URL of every pad is publicly visible by anyone that knows the URL. Launchpad is a demo platform, so you should not use it to run critical production code you rely on, or anything you want to keep secret.

## Secrets

We’ve included a feature called secrets that lets the owner of a pad put in API keys and other sensitive information which is hidden from everyone else. So while anyone can read the code and call the endpoint, they can’t extract database passwords or API keys. TK add information about how we do this.

Adding a secret can be done by clicking the "secrets" button in the lower right of the launchpad dashboard and enter a key and value for your secret. 
![launchpad access key modal](http://i.imgur.com/kKUUSuy.png)

Accessing your secrets within your code can be done by exposing the secrets or headers object in context. See the [CurrentWeather](https://launchpad.graphql.com/5rrx10z19) examples for a live example.
```js
const resolvers = {
  Query: {
    user: (root, args, context) => {
      return fetch(`https://api.netlify.com/api/v1/user?access_token=${context.secrets.ACCESS_TOKEN}`)
      	.then(res => res.json()).catch(err => console.log(err));
    },
  },
}
...
export function context(headers, secrets) {
  return {
    headers,
    secrets,
  };
};
```

## Fork to edit

If you’re viewing someone else’s pad, you can log in and click “Fork” to make a copy that you own. In the process, any secrets will be deleted, and you will need to provide your own values for those keys. For example, if you fork the MongoDB example, you’ll need to go to a MongoDB hosting provider like mLab, create a database, and put in the new database URL.

## Sharing examples

For  the initial launch, we don’t have a site for browsing and sharing pads. We hope people will discover pads through links around the internet, from package READMEs, documentation, and tutorials. However, to make it a bit easier we’ve put up a repository where you can send a PR with a link and description for your pad to share it with the world! See it here:

## Code

The code you write in Launchpad runs in Node.js 4 on Auth0 Extend (you might know this as Webtask), and is compiled with Babel so that you can use modern JavaScript features. So that you don't have to write any server boilerplate, you can just export the following things from the module:

### `schema`

A GraphQL.js schema object. You can write this directly by hand, or use a package like `graphql-tools` to generate it.

### `context`

A function that generates a context object from the request headers and pad secrets. It can return an object or a promise that resolves to an object.

### `rootValue`

An object that is passed in as the parent object in the resolvers on the root Query and Mutation types.

## npm dependencies

You can use any dependencies from `npm` in your pad. Just import from any package, and when the code is deployed our server will automatically include the newest versions of those packages for you.
