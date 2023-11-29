---
title: "Deploying a Next.js Static Site on DigitalOcean's App Platform"
datePublished: Wed Nov 29 2023 09:08:29 GMT+0000 (Coordinated Universal Time)
cuid: clpjjobwx000a09l43o94fz1j
slug: deploying-a-nextjs-static-site-on-digitaloceans-app-platform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1697723729147/445ef4c0-4505-4dfe-9943-970d23e342d8.webp
tags: digitalocean, nextjs, app-platform

---

Wondering if deploying a Next.js static site on DigitalOcean can deliver a seamless experience? 😂

I was assigned the task of deploying a static site and initially, I assumed it would be as simple as deploying a React Native app. I diligently followed the steps outlined in the [documentation](https://docs.digitalocean.com/developer-center/deploy-a-next.js-app-to-app-platform/#deploying-nextjs-as-a-static-site).

After successfully completing the build process, I eagerly clicked on the URL, only to encounter a frustrating 404 error. It was then that I delved into the build logs to troubleshoot the underlying issue.

# **Identifying the Issue**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697629294087/c5bb8773-1cf2-437f-8cc4-d87b80aee7b1.png align="center")

The root cause of the problem lies in the absence of a clear definition for the output directory of the static site. As a result, it was searching for static files and, by default, resorting to the standard 404 error document provided by the App Platform.

# **Finding a Solution**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697628639572/ef1d6b2d-a2d7-4fca-b56a-bb40a6087942.png align="center")

To resolve this issue, head to the settings and make a modification in the [appsec](https://docs.digitalocean.com/products/app-platform/reference/app-spec/) file by adding the line `output_dir: /out`*.* Don't forget to clear the build cache before initiating a redeployment.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697628363723/924e787c-f1e6-4b3a-8ebf-ffa2d0851adc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697628561155/0202e6f5-6f2d-4a63-94ba-d9b66d80afe9.png align="center")

The build process concluded successfully, and the static files were exported to the designated `/out` directory.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697629013326/182dfe9b-726a-4510-a0ef-c525ba76943f.png align="center")

# **Managing URL Path Rewrites and Redirects**

If your application involves URL path rewrites or redirects, consult the [documentation](https://docs.digitalocean.com/products/app-platform/how-to/url-rewrites/) for the specific steps. You'll either need to manually configure these routes or include them in the `appsec` file under the `ingress rule` section.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1697630547604/bae9f6c4-fa29-45ac-a5ae-a5692d157bdd.png align="center")

# **Handling Domain Management in the App Platform**

For guidance on effectively managing domains while using the App Platform, refer to the [documentation](https://docs.digitalocean.com/products/app-platform/how-to/manage-domains/), which offers a clear and straightforward guide.

## **Conclusion**

Deploying a Next.js static site on DigitalOcean's App Platform is easy when you have the right guidance. This guide is here to help you make the process smooth and enjoyable. Additionally, the GitHub integration automates the build and deployment process, making it even more convenient.

App platforms offer many benefits like simplified deployment, automatic scaling, managed infrastructure, security features, CI/CD support, isolation, monitoring, and more. They boost developer productivity and reduce costs. The choice of the right platform depends on your project's needs and your organization's goals.