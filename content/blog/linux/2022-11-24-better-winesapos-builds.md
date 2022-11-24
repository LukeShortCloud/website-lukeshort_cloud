---
title: 2022-11-24
date: 2022-11-24 01:00:00
categories:
  - operatingsystems
tags:
  - archlinux
  - arch
  - manjaro
  - manjarolinux
  - winesapos
---

[Back to Linux blog.](../#linux)

# Better winesapOS Builds

{{< image src="../../images/blog/linux/2022-11-24-better-winesapos-builds.jpg" alt="Library full of books" set="fit" >}}

[winesapOS 3.2.0](https://github.com/LukeShortCloud/winesapOS/releases/tag/3.2.0) is here with our most requested feature: a minimal image weighing in at only 7 GiB. Thanks to our amazing community, we get to work together to shape the future of Linux gaming.

With this grand new release, I wanted to take some time to reflect about how I have historically built my operating system and how I'm working on improving it. As a quick recap, winesapOS is a passion project of mine. It started off as a simple way for my best friend (who owns a Mac) to play Halo with me. The solution was straight forward, in theory: create a portable Linux operating system.

winesapOS has become a large project with a small cult following. To that end, I can count on one hand the number of contributors we have for our project. I've had to re-evaluate both the architecture and costs for server and build infrastructure. I've also switched to Starlink so there are some challenges that come with satellite internet. As they say, constraints are the gateway to ingenuity. How are we addressing these things?

- **Slimming down.** We used to run our repository in a highly-available 3-node environment. However, we can tolerate downtime, the resource usage was small, and I could no longer justify the cost. Due to these reasons, we had to slim down to a single node. If demand ever became an issue, I would look into object storage or burst computing service offerings.

- **Reliable builds.** We now force Pacman to use the "wget" command for all package downloads during the install process. This is great for slow or unreliable internet connections. The default Pacman behavior is to fail if a connection is too slow. This has helped me with my satellite internet and will help others in rural areas, too. This even extends beyond the builds. Release images now use "wget" as well so that everyone can benefit from this change.

- **Fast builds.** I recently built out a Squid proxy server to help cache files. winesapOS now supports using proxies. My Squid proxy is setup to cache both HTTP and HTTPS content. For HTTPS, I essentially do a man-in-the-middle attack against myself. Don’t worry, for security and safety reasons, I don’t do this for release builds on GitHub. Don’t believe me? Check the full build debug log in your `/etc/winesapos/winesapos-install.log` file. With my proxy, I was able to get our secure image build down from almost 2 hours to 28 minutes! About 10 minutes of that is refreshing the Arch Linux keyrings (this is required to workaround issues if the Arch Linux ISO keyring is out-of-date). Essentially, our build is around 18 minutes now! This really helps when I may need to do multiple builds every night!

- **GitHub Actions.** Thanks to my friend from the community @Thijzer, we now have automated testing! winesapOS was built from the ground-up to do test-driven development. This takes it a step further by testing every git commit via a GitHub Actions continous integration (CI) pipline. Long-term, I want to also include continuous delivery (CD) so that it can publish release images instead of me having to manually download them. This has saved a lot of time already and is paving the way for the future.

- **Reliable hardware from Intel.** Intel graciously gifted me with a handful of Intel Optane drives to use. Not familiar with Intel Optane? These are data center grade drives that work wonders to remove I/O bottlenecks. I have many use-cases for them but I wanted to start off with winesapOS since I’m dealing with that on a daily basis. I ran a large amount of builds on these new drives and, ironically, the time to build the image was exactly the same: 28 minutes with a proxy. This clearly tells me that the bottleneck is not the drives but in my build script. It is very serial/linear. Moving forward, I will be focusing on parallelizing our build. I already have a few things in mind and we’ve already laid down some ground work to eventually support this. In the meantime, what benefit can these fancy Intel drives provide? If I were to keep using them as build drives, they would last a lifetime. Maybe even two or three lifetimes. Better yet, I intend on putting these drives to better use elsewhere such as my Squid proxy and/or ZFS file server. These would greatly benefit from the incredibly high IOPS.

    - Let me put the lifespan in perspective. Each asterik represents 5 years of use for building winesapOS (assuming I use 100 GB per day):

        - \* = My old NVMe drive.
        - \*\* = Intel Optane 800P 120 GB.
        - \*\*\*\*\*\* = Intel Optane P1600X 120 GB.
        - \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* = Intel Optane 905P 480 GB. This is not a typo! The TBW lifespan on this drive is incredible. I've never seen a drive with such endurance!

    - Most Intel Optane drives are on sale right now for the holidays! Check out all of the various deals going on at [NewEgg](https://www.newegg.com/p/pl?N=50001157%20100011693&d=intel%20optane&isdeptsrh=1).

Have any other ideas for how we can improve winesapOS? [Get in touch](../../#contact) to let me know!
