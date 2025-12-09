---
title: "Week 10 Worklog"
date: "2025-11-10"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:
* Test static web hosting with S3 and CloudFront.
* Set up a real domain and CI/CD flow for the React-based dashboard.
* Explore audio modding for World of Tanks using Wwise.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | Built a basic test website to validate S3 static hosting and CloudFront: <br> &emsp; + Created an S3 bucket configured for static website hosting. <br> &emsp; + Deployed a simple HTML/CSS site to S3. <br> &emsp; + Set up a CloudFront distribution pointing to the S3 bucket to test global delivery and caching. | 10/11/2025 | 10/11/2025 | AWS S3 & CloudFront docs |
| Tue | Purchased a domain from Porkbun and connected it to the site: <br> &emsp; + Registered a custom domain and configured DNS records. <br> &emsp; + Pointed the domain to the CloudFront distribution. <br> Started implementing the dashboard as a React application to later deploy on S3/CloudFront. | 11/11/2025 | 11/11/2025 | Porkbun docs, React docs |
| Wed | Uploaded the React dashboard project to GitHub and set up automation: <br> &emsp; + Pushed the dashboard source code to a GitHub repository. <br> &emsp; + Configured a GitHub Actions workflow to build the React app on push. <br> &emsp; + Automated upload of build artifacts to the S3 bucket and CloudFront cache invalidation after each deployment. | 12/11/2025 | 12/11/2025 | GitHub Actions docs, AWS CLI / SDK docs |
| Thu | Researched how to mod audio for World of Tanks using Wwise: <br> &emsp; + Read through the Wwise modding guide and World of Tanks mod documentation. <br> &emsp; + Studied the required folder structure, banks, and sound event workflow. <br> &emsp; + Took notes on how to package and load custom audio mods into the game. | 13/11/2025 | 13/11/2025 | [Wwise mods creation PDF](https://wotmod.mtm-gaming.org/resources/Wwise%20mods%20creation.pdf) <br><br> [World of Tanks Wwise mods page](https://wgmods.net/4894/) |
| Fri | Continued audio modding research and experiments for World of Tanks: <br> &emsp; + Tried creating or editing simple sound banks in Wwise following the guides. <br> &emsp; + Experimented with building a test audio mod and integrating it into the game’s mods folder. <br> &emsp; + Documented steps and issues to reuse later for a more complete sound mod. | 14/11/2025 | 14/11/2025 | - |

### Week 10 Achievements:

* Successfully set up a simple S3 + CloudFront static website and then attached a custom Porkbun domain to it.
* Built and automated deployment for a React-based dashboard using GitHub Actions, S3, and CloudFront invalidation.
* Gained foundational knowledge of World of Tanks audio modding using Wwise, including documentation, folder structure, and basic mod packaging.
