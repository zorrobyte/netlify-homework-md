# Getting Express working

I'm tempted to just go ahead and provision a droplet on DigitalOcean and build my own PostgreSQL node, but let's hold off on that and remain curious as to what Netlify offers (and the best practice for the platform).

We've established that the `html` files are indeed being served from the `public` directory as expected.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Now as for the `express` backend, which is located in the `backend` [folder of my repo](https://github.com/zorrobyte/blog-js-project/tree/netlify/backend).

{% embed url="https://docs.netlify.com/frameworks/express/" %}

Following along:

1. Netlify Dependencies [https://github.com/zorrobyte/blog-js-project/commit/97fe7e0cf02316d47b6bd965980164b29f663a28](https://github.com/zorrobyte/blog-js-project/commit/97fe7e0cf02316d47b6bd965980164b29f663a28)
2. Going down the rabbit hole....
3. Dang, it seems there will be quite a bit of project reorganization and some refactoring
4. Creating the `netlify>functions` directory
5. Wait! `The default directory is YOUR_BASE_DIRECTORY/netlify/functions. You can customize the directory using the Netlify UI or file-based configuration.` Epic. Changing the `functions` dir to my existing `backend` directory
6. Reviewing [https://docs.netlify.com/configure-builds/file-based-configuration/](https://docs.netlify.com/configure-builds/file-based-configuration/) for express configs
7. Deployed initial `toml` with some assumptions as to the needed config [https://github.com/zorrobyte/blog-js-project/commit/fce26854e2e6ab2297d5bffbdfa99e2fa95ecf79](https://github.com/zorrobyte/blog-js-project/commit/fce26854e2e6ab2297d5bffbdfa99e2fa95ecf79)
8. Deployed a copy of my existing `server.js` as a starting point to `api.js` [https://github.com/zorrobyte/blog-js-project/commit/2b1c44989ac1cdc8f4a73d500d471f391b30ac98](https://github.com/zorrobyte/blog-js-project/commit/2b1c44989ac1cdc8f4a73d500d471f391b30ac98)
9. First pass of a refactor to `serverless` [https://github.com/zorrobyte/blog-js-project/commit/bb196099437ccd03f39e262d4ca990f351c8295b](https://github.com/zorrobyte/blog-js-project/commit/bb196099437ccd03f39e262d4ca990f351c8295b)

Let's pause and check our work so far!

***

It looks like CI is wicked fast and is deploying my code very quickly, nice!

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Our first error!

<figure><img src="../.gitbook/assets/image (25).png" alt="" width="563"><figcaption></figcaption></figure>

`Oct 11, 09:18:32 PM: ERROR Uncaught Exception {"errorType":"Error","errorMessage":"/var/task/backend/node_modules/bcrypt/lib/binding/napi-v3/bcrypt_lib.node: invalid ELF header","code":"ERR_DLOPEN_FAILED","stack":["Error: /var/task/backend/node_modules/bcrypt/lib/binding/napi-v3/bcrypt_lib.node: invalid ELF header"," at Module._extensions..node (node:internal/modules/cjs/loader:1460:18)"," at Module.load (node:internal/modules/cjs/loader:1203:32)"," at Module._load (node:internal/modules/cjs/loader:1019:12)"," at Module.require (node:internal/modules/cjs/loader:1231:19)"," at require (node:internal/modules/helpers:177:18)"," at Object. (/var/task/backend/node_modules/bcrypt/bcrypt.js:6:16)"," at Module._compile (node:internal/modules/cjs/loader:1364:14)"," at Module._extensions..js (node:internal/modules/cjs/loader:1422:10)"," at Module.load (node:internal/modules/cjs/loader:1203:32)"," at Module._load (node:internal/modules/cjs/loader:1019:12)"]}`

{% hint style="warning" %}
Feedback: 300 characters for the Docs Chatbot seems a little low, but still, it got me pointed in the right direction\
<img src="../.gitbook/assets/image (26).png" alt="" data-size="original">
{% endhint %}

> To resolve this issue, you might need to ensure that the `bcrypt`library is correctly installed and compiled for the environment where your function is running. If you're deploying your function on Netlify, you might need to use a version of `bcrypt` that is compatible with AWS Lambda's runtime environment, as Netlify Functions run on AWS Lambda under the hood.

:boom: Called it, I knew that Netlify Functions smelled like AWS Lambda

I'm just going to uninstall `bcrypt` and use `bcryptjs` instead. Commit:

{% embed url="https://github.com/zorrobyte/blog-js-project/commit/766eb060e27565ee804f832a4f95bff9425c97b3" %}

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

There we go! We are ready for PostgreSQL!

But wait! Let's refactor our front end API calls to use the new backend:

{% embed url="https://github.com/zorrobyte/blog-js-project/commit/73d0ac7cd1a178e2ee181920cea03e17ae84ccb3" %}

(And yes, I could use DRY and set my API endpoint to an env var or otherwise variablize it (I'd recommend doing just that to a customer as a best practice), but that's for a future PR)
