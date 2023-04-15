---
title: "Android App Internal Testing On Google Play Console"
datePublished: Tue Sep 20 2022 12:18:06 GMT+0000 (Coordinated Universal Time)
cuid: cl8a5wm6g000909l56fqjf3nl
slug: android-app-internal-testing-on-google-play-console
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1663675887992/oCrQRfpu9G.jpeg
tags: mobile-apps, android, hashnode, mobile-development, 2articles1week

---

Today it has become simple to develop your own apps, test, and publish the app on Google Play store using the specific tools and features. Before a new version of your app is made public it's a good choice to have it tested within your testing teams.

![play](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ejlpfxzp66ne0sam44yh.png)

In this article, we’ll learn how to test Android apps via Google Play Store. Note that there are different types of testing tracks available which are listed below but I will be discussing **_Internal Testing_** because deployment is much faster than the other tracks.
- Internal Testing
- Open Testing
- Closed Testing

For more insight about setting up an open, closed, or internal test, refer to the [Play Console Help documentation](https://support.google.com/googleplay/android-developer/answer/9845334?hl=en&visit_id=637989170271487669-1265562062&rd=1)

Google play console provides Internal Test to create releases for testers which is great for quick iteration during development, catching bugs early, and getting fast feedback from your team. The key is to directly distribute a test version of your app to a group of users. This could be product managers, QA, designers, or whoever can identify issues and give early feedback in your development process. With the Internal test track, multiple lists of testers (100 testers) can be created, which you assemble by email address (Google email account).

Testers don’t need a special app or permissions to install your test builds with the internal test track. The first time you deploy, it may take up to 48 hours. However, after that, your builds will be available to your testers within a few minutes fast and easy. All they need is a link, which you’ll find in the Play store Console.

![playstore-link](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yymu0lyecd509084fdn1.png)

After accessing the link, users can access the pre-released app via Google Play Store. If users have opted into pre-release testing, they can always see the latest build in Google Play Store.

![google-play](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bk1g2ofyhgd8pf3ocolh.jpeg)

## Conclusion

With the Internal Testing track, Google PlayStore provides a simpler app distribution system straight from the development phase, gets feedback from groups of testers, integrates policy checks, takes advantage of services like Pre-Launch reports, and creates a CI/CD pipeline.