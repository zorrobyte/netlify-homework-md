# Getting everything working

<figure><img src="../.gitbook/assets/image (15).png" alt="" width="439"><figcaption></figcaption></figure>

I seem to be having issues with the backend as my endpoints are returning 404, I'm also not getting logging output from my api function

<figure><img src="../.gitbook/assets/image (32).png" alt="" width="563"><figcaption></figcaption></figure>

We've done a whole lot so far, I'm about 3 hours in, so I'm going to take a break and get a snack :pizza:

***



Good time to reference the [Question Key](../introduction/question-key.md)

{% hint style="info" %}
Briefly explain one challenge you experienced in setting up this site and how you overcame it.
{% endhint %}

Apologies, but this won't be `brief` as we'll be debugging a full-stack application, but I'll aim to troubleshoot efficiently.

Alright, so let's start with the basics. Netlify is claiming my Endpoint is [https://ross-blog-project.netlify.app/.netlify/functions/api](https://ross-blog-project.netlify.app/.netlify/functions/api) - let's sanity check that.

Adding a simple test route to my `app.js` and pushing

<figure><img src="../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

Bummer

```
Cannot GET /.netlify/functions/api/test
```

Ahhhh what do you know?

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="479"><figcaption></figcaption></figure>

Obviously since we are using redirects, my endpoint would be at `api`

```
[[redirects]]
  from = "/api/*"
  to = "/.netlify/functions/api/:splat"
  status = 200
  force = true
```

{% hint style="warning" %}
Feedback: It would be nice if the UI showed my actual API endpoint as it seems users stumble upon this often looking at the netlify forums

![](<../.gitbook/assets/image (4).png>)
{% endhint %}

You know how I said I should have used DRY and stored my endpoint in a variable or env variable earlier? Ha.

{% embed url="https://github.com/zorrobyte/blog-js-project/commit/e16e4663f05c3379b86debbf04b1f6a93b7c898b" %}

HEY IT ALL WORKS! :raised\_hands:

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="563"><figcaption></figcaption></figure>

Go checkout my full stack blog app I created and we learned how to deploy on Netlify at:

{% embed url="https://ross-blog-project.netlify.app" %}
