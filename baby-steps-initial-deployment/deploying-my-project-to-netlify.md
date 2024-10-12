# Deploying my project to Netlify

I built a full stack blog example app to brush up on my NodeJS/Express/PostgreSQL skillsets just the other day, you can find the Git repo here:

{% embed url="https://github.com/zorrobyte/blog-js-project" %}

Let's get this deployed to Netlify!

***

My first step was to link my repo to Netlify

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

I'm a little currently mystified as to how to configure the Build settings, however, docs to the rescue!

{% embed url="https://docs.netlify.com/configure-builds/overview/#basic-build-settings" %}

It seems as if my app is a `monorepo` as my [backend ](https://github.com/zorrobyte/blog-js-project/tree/main/backend)and [public ](https://github.com/zorrobyte/blog-js-project/tree/main/public)folders are at root level, let's figure out how to configure Netlify for this, then!

And as for `express`&#x20;

{% embed url="https://docs.netlify.com/frameworks/express/" %}

Alright, so Netlify Functions, kind of like AWS Lambda, right? Before we go much deeper, let's see what's included in my plan before we get too far down the rabbit hole.

The "Starter" should indeed have "Dyamic serverless functions", let's check out what "Background functions are" just for full optics.&#x20;

> Netlify’s Background Functions provide an option for serverless functions that run for up to 15 minutes and don’t need to complete before a visitor can take next steps on your site. For tasks like batch processing, scraping, and slower API workflow execution, they may be a better fit than synchronous functions.
>
> ### [#](https://docs.netlify.com/functions/background-functions/#how-background-functions-work)[https://docs.netlify.com/functions/background-functions/](https://docs.netlify.com/functions/background-functions/) <a href="#how-background-functions-work" id="how-background-functions-work"></a>

I should be golden, then!

***

Back to initial build configuration.&#x20;

{% hint style="warning" %}
Feedback: I just created a new branch while thinking about "Branch to deploy" and I'm a little bummed there isn't a refresh button. No worries, refreshing the page and selecting my branch did the trick.

![](<../.gitbook/assets/image (18).png>)
{% endhint %}

`The directory where Netlify installs dependencies and runs your build command.`

Looks like that should be my `backend` folder as that's where my `package.json` and `package-lock.json` are located.

<figure><img src="../.gitbook/assets/image (19).png" alt="" width="280"><figcaption></figcaption></figure>

Or not, as this also populates my `publish directory` when that should be /public.

Let's just go for it and see what breaks, sometimes the best way to learn (as long as you aren't in production)!\
\


<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

Hey, look at that! Netlify just seemed to work!

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

***

Now the more complex part, getting express working and configuring a PostgreSQL server.
