# Configuring PostgreSQL

Now that express and the frontend (should) be all configured, we need a datastore. Googling `netlify postgresql`

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="563"><figcaption></figcaption></figure>

Dang. While I have a ton of experience supporting MongoDB at scale thanks to my time at Compose/IBM - let's play along as a customer who loves PSQL and see where the journey takes us!

Looks that while Netlify doesn't have a first party PSQL provider, we can just use PSQL self hosted, or any other hosting provider without any `magic`.

{% embed url="https://answers.netlify.com/t/cannot-connect-to-postgresql-database/84165" %}

Looks like it's time to slap a PSQL server on DO

<figure><img src="../.gitbook/assets/image (9).png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="danger" %}
I chucked this on a $4 a month node, so a minior expense. Usually I'd setup HaProxy, failover, backups, etc, maybe even deploy using helm with kubernetes with autoscaling and regional failover if I was being super serious and going to production, but I'm just throwing this together for the homework. Please excuse my less than impressive database layer for the sake of time and this use case.

Of course, I would recommend several best practices to a customer going into production with Netlify with PSQL, or any other database including security and disaster recovery suggestions.
{% endhint %}

Getting `psql` installed, etc

<figure><img src="../.gitbook/assets/image (10).png" alt="" width="435"><figcaption></figcaption></figure>

{% hint style="info" %}
Note to self, psql server info is in Notes
{% endhint %}

Sweet, PSQL is up and running, firewall rules are deployed, and psql configs are allowing connections.

<figure><img src="../.gitbook/assets/image (12).png" alt="" width="563"><figcaption></figcaption></figure>

Now to wire up my express backend from netlify to my psql backend!

***

The first thing we need to do is get rid of our .env as to not leak any secrets on Github. Pushed public secrets is `very very bad`

<figure><img src="../.gitbook/assets/image (13).png" alt="" width="563"><figcaption></figcaption></figure>

Nice, looks like Netlify has a secrets manager, let's get it working!

{% embed url="https://docs.netlify.com/environment-variables/secrets-controller/" %}

That was easy as just pasting in my .env file, epic! Let's remove the .env file completely:

{% embed url="https://github.com/zorrobyte/blog-js-project/commit/e4e93a44367084fc6c862783e87a047e9776e548" %}

From the docs, it seems like this should just magically work now, so let's update our env vars to our actual PSQL server details

<figure><img src="../.gitbook/assets/image (14).png" alt="" width="563"><figcaption></figcaption></figure>

