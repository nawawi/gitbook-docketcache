---
description: Understanding the different types of hosting for WordPress.
---

# WordPress Hosting

Putting a WordPress website online means having hosting. There are numerous types of hosting to consider, each of which has its benefits, pitfalls and target groups.

{% hint style="success" %}
This article originally from [Andrew Killen Note](https://www.facebook.com/notes/andrew-killen/understanding-the-different-types-of-hosting-for-wordpress/10155584723456701/).
{% endhint %}

## Free Hosted Service

[wordpress.com](https://l.facebook.com/l.php?u=https%3A%2F%2Fwordpress.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT052cjykSEMw60mQLiVE1Gez2d-I9GY4n-nFRiDtyDdyA_XzxXhAkKPEXmigd13QEfD1qtMW4UXNaS5CLxPSwEfYtPUfjCgraCpAbPCqSTCqU1t2JnlkCLPSDc) is a free platform that is maintained and owned by [Automattic](https://l.facebook.com/l.php?u=https%3A%2F%2Fautomattic.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT09ZpuYBhQGmQbxcYaeWYEeYb6JnIWo72g53eU9QRo6oX4hSMdz0W1uavVr-PhhfBp9Ha64fbslix7uN5ssUYHIKHuSQe-AzHb-wKRQxUMShErbnuMqFcI1Lpg). Automattic is owned by [Matt Mullenweg](https://www.facebook.com/matt.mullenweg/?eid=ARBP_zBUaXi0VPAq3ZrF_FOo5UbEc5HCtHOV3kYT8LaT78t-Yc57Ks4bhO0srM_L0_eslvRclWnfaz0v), who is the benevolent dictator for life of WordPress. Hosting on WordPress.com’s platform is very easy to setup, but is limited in the plugins & themes you can use, and force your site to carry advertising in order to pay for this service.  
  
Due to WordPress’s multisite capability, others are offering this service, but I’m sticking to WordPress.com for this overview.

#### Pro's

* Free.
* Good platform.
* Can use your own domain name.

#### Con's

* A limited number of themes and plugins.
* Forced advertising on site.
* Automattic can pull the plug on your website without warning.
* Things like backups and restores are not feasible.
* Limited to community support and 3GB max data space.

#### Target Audience

People who just want to start blogging.

## Cheap Shared hosting

I am sure you have seen it, $1 per month hosting or similar, maybe promising unlimited disk space, unlimited bandwidth, unlimited email accounts. The only way that this can be achieved is by 2 things.

1. Putting as many websites as possible on one server \(we’re talking thousands\).
2. Offering very poor support.

By having many thousands of websites on one server, there will always be contention for processing power, disk access, and database calls. You can be sure that the site will at some point start to run slow, maybe be out of the water for days on end.

Mostly the host will not care. Offering the hosting as such a cheap means that they will not have a lot of money left over to pay for qualified staff to manage and maintain their systems. If the server costs 80 cents off of the dollar per month to keep running, then they can only make a profit on the 20 cents.

Take in to account the cost of the financial transaction and offices, they maybe down to as low as 5 cents per month for support. So don’t expect the best of the best technologist, quick answer times on the phone and chat. Also don’t expect the latest and greatest to be installed on the server, i.e. latest version of PHP. They just can’t afford to do it for you. I am assured by the people at [Hostinger](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.hostinger.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT3O2XHqpQeG_2TOU3ZJtXkGbEd52DlyOqKzPVK9I-osbo8kdINt8lrIeyCzQvvgxxZoggshYAmCzbWVpqvBzhjFcQppwk4HZVBaAjNDzHT8O6KLs9x9AtoyRJY) that this is not always the case, and they specialize in this part of the marketplace.

#### Pro's

* Super cheap.
* You can have the themes and plugins of your choice.
* An admin panel where you can administer your website\(s\).
* Possible SSL via LetsEncrypt for free \(not guaranteed\).

#### Con's

* May not be the latest software or hardware.
* High chance of hackers breaking in from one of the other sites hosted on the server.
* A Server might often run slow.
* Support might be hard to get.
* A Server might be out of action for days at a time.
* Every problem will be resolved with an upsell to better hosting.
* Definitely not suitable for e-commerce.

#### Target audience

Those that do not value their business enough to invest in it, or just need a very basic web presence.

#### Recommendation

Don’t do this if you are in any way professional about your business or online presence. Check how others have found your hosting company of choice before signing up.

## Professional Shared Hosting

Professional shared hosting usually starts at around $20 per month, and like cheap shared hosting makes use of putting many websites on one server. However, that is where the similarity ends. Companies like [siteground](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.siteground.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT3h8qt1qbNFe9h_W96TmyYljk5xSRHzicvzkd1bMy_xCxS8Edq3zcYLT3WgKGdvBTmRBw2hBeVsMJlM52EQ88BoeyUzpzDNONoo9t-hu1etjAOg5BFNXSwZJIQ), [WPengine](https://l.facebook.com/l.php?u=https%3A%2F%2Fwpengine.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT09zcsNhUgK_2F3enAJudjvWfU-pNk3FVeRRGFWlgJBqst66gRRKBesw8DDHYPJbJFVRdZbxB7jk5IO43MlXElmSPWYVbTMMYXttXhAJXp7JULjYcZWIZ--JNw), [Flywheel](https://l.facebook.com/l.php?u=https%3A%2F%2Fgetflywheel.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT2FVm7e44Vjx4Kie-ca64FBMdKMEPDSv1XMflAYq_TEAm7n8tprXN7yIalWBHeJOdq0NGGZvlcgxyLuyO6euvTQxUCUq-9sPV2Yem6ERsBjHQSnVj2Rf54l_J8), [WordPress.com](https://l.facebook.com/l.php?u=https%3A%2F%2Fwordpress.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT31u0WJPrVFanvBedD8KoxkogEnU0rHgavqUMsASwTVAZ7EgUy-EleA6-7JR2NbfqCrPYnW9hulkJSbsRgeOD3dK6AHInmuq01RtKY-tRbcr3LfSrDGtJsR7w0), to name but a few target this part of the marketplace \(a more comprehensive list [here](https://reviewsignal.com/webhosting#tab6)\).

Usually, they offer a very structured hosting package, where there are limits per month for what bandwidth and processing can be used so that they can be sure that the server will not be overworked. Also, they will often have limits on what plugins can be installed.

This is based on, for example, either not needing the caching plugin to be installed as their infrastructure takes care of it, or that they know a certain plugin uses far too much processing power or has a high chance of the site being hacked if it is used. If you do get problems and need support, you can be sure in this package that they will be prompt in answering, and will less often offer up-sell as a way to resolve problems.

#### Pro's

* Usually free SSL cert.
* Often free domain name in the package.
* Quality support staff.
* Good administration tools.

#### Con's

* Limited resources are available.
* Sometimes upsell will be offered as a fix resolution.
* Possible that the website will be out of action if you have a viral post and you have not worked with the hosting company to deal with the extra processing/bandwidth resolution.  
* Might not have SSH access.

#### Target Audience

Those that value their online presence. This is a suitable business solution for the lower tier.

#### Recommendation

This is a good first start for a website platform. Consider the benefits of your chosen company before buying, for example, WPEngine pride themselves on their security so are good for E-commerce, Flywheel is aimed at developers and making their life easier \(good tools and staging\), Siteground do well with caching and hosting in general, few complain about them.

## Self Managed VPS \(Virtual Private Server\)

A VPS is a server that you own and can put on there what you like. Usually, they are based on top of Windows or Linux, for WordPress it’s always better to go the Linux route. Usually, when buying a VPS, you pay for the number of processors you use and the amount of memory wanted, lastly the amount of disk space needed.

There are many companies that are working in this space, some of the more popular ones are [Linode](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.linode.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT3I1KCZocInW_Q8d8510t93aFK6QHV_VWGqpbSKr9BHqQbRT21ugDhruWoO3WO8njDTU4mCMczsnEhG8EBEv6IeYDDMqR8jjCPsw-IXgOIGq6idB201MLKtzsg), [Digital Ocean](https://www.digitalocean.com/?fbclid=IwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME), [Vultr](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.vultr.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT26vsx1EMmBR2mtHRyYA5y48dLvABSTvnefRM0Juewhn9y9VOMQ9zLxubclMf1f4pSBST4Y4d6wfulmBkmB25N_esrJw41-FNBfH4ll7kkKgqBz8yZZtCk5dlI) [Amazon AWS](https://l.facebook.com/l.php?u=https%3A%2F%2Faws.amazon.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT2NKo0AonhDRmYmwb90SVXlJXoLokI1RM0ahL_xgfRVY8m_VM8P3NKaKscqLpUqi8wtAA_85URsdiw6X0873pm_RUg_ypQszimWRVJDrq-0exUeK0a9RsMjlFKZR6xx_tEY-seG), [MS Azure](https://l.facebook.com/l.php?u=https%3A%2F%2Fazure.microsoft.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT1HN46HrVlSPNC6pxhMt5p5cOXK5a06_NNY_Yom5m9Wa5CGHz9SiT9MRhDLVydrjwMNaCsfOmfzIFWLG0iTzugtVrjeEH-ruB7tC8U1oXgzDz6PxShCTLEAZgw) etc.. And start as low as $5 per month. The downside to this type of hosting is that you own the machine completely if it breaks it’s down to you, you manage the security, the operating system, the server configuration, email configuration and so on. If you are not a DevOps person, do not take this route.

#### Pro's

* Cheap.
* Versatile.
* Can install whatever you want.
* Your choice of the web server.
* Extremely fast.
* You can choose where in the world the server is.

#### Con's

* You own it, manage it, maintain it, secure it.
* Ownership takes time.

#### Target Audience

Those that are as happy with Linux as they are with PHP, Varnish, NginX/Apache, REDIS, Memcache or PostFix. If any of those words scare you, do not choose this option.

#### Recommendation

These are great hosting for test sites, and for those that really are on top of their technology stack. Remember that if you own a large site, you will also need staff to support it. AWS & Azure are more difficult than other hosting packages to understand exactly what it is you will need and pay for, consider employing a consultant to help define this, [RackSpace](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.rackspace.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT3NYmg7x0nqUu2j9rgS3ooaHcrwZ-kf3BwBa7B7sNQEu7Zl8uLoeV6hxj_tSYDR_pRJ_PIFjN86-gHt1pNYVrYUWui9Bc2X0G31jQwapiOkEwSIqYGacKZMDeo) are very experienced at this.

## Managed VPS

There are a growing number of companies that manage the VPS technology for you. They do this by installing a stack that they own and know intimately and can support fully. Perhaps the best-known name in this arena at the moment is [cloudways](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.cloudways.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT2F6rGLiyJnRCU30lFg1BSkt6c17cck4NuWsSYZstE0rzLqG8a6QOnU_2P4T3KebwZzpt_M_9rjeX_Mz7nTx2M9agMyhVch9WCD8mHn6IZy-1YN2nG-zCBiYdk), where they offer many different platforms for hosting, with same technology stack on each. 

So you can choose if you want Digital Ocean or AWS, and leave them to do the rest. If you need to be installing a specific technology stack, this is not for you. But if you are just looking for quality hosting at reasonable prices then this is a great option. Be aware that if you choose this path, you might not get such things as SSH access.

#### Pro's

* Great value for money.
* Can grow to meet your bandwidth needs instantly.
* You do not support it.
* Proven stack that works.
* Security is managed by someone else.

#### Con's

* Possibly no SSH access.
* Cannot install whatever you want \(i.e. Fail2Ban\).
* You have to have a bit of web hosting savvy when making the purchase to speck out the right system.

#### Target Audience

Those that want a professional level of hosting that can grow without worry and will not have to employ own support staff to manage it. If you just want a WordPress website online and don’t care about the underlying technology, this is one of the most cost-effective solutions.

#### Recommendation

If you are doing anything special, before choosing this solution make sure that your technology stack will work and you do not need things that the managed VPS hosting company can’t deliver.

## Managed Hosting

We are now entering the upper tiers of hosting. This is the choice of the professional, large business, or those that want to be always online with high availability and strong security. There are many companies that focus on this arena, [WPEngine](https://l.facebook.com/l.php?u=https%3A%2F%2Fwpengine.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT31y5IisnoM9GquKcsxedmaGAl8lAr48JxF8TsJczBNch9cN9lMchbM6dxcJUyYXjEt86h7F16JkJOIy_WKtodLo0KToCJ2FE6IAM1ZQtbuO1eFPmIfslKM0G8), [Kinsta](https://l.facebook.com/l.php?u=https%3A%2F%2Fkinsta.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT0aM7LHk14SI4NP6tJolRbBmF7ZpvoUDLrthh9ku1GkEw7HVfpx3LceVuF2OWNg46aZ5n4ZTt1_z2CxO8gA0AOnprW46OrfJahK6mkwxr3be4UJC95gjLrt0Xg), [Pagely](https://l.facebook.com/l.php?u=https%3A%2F%2Fpagely.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT3hKe4slzefnQgmNobGMPmIn6pAflRyKHCjWqSM2wYv06Tr129mWpkEC7QFzhjCnV-4HUaeGs92_CeUUpA1vUJKZyu6NYehQU-yOCycsp9MNyK75AiKfAY7GBM), [Flywheel](https://l.facebook.com/l.php?u=https%3A%2F%2Fgetflywheel.com%2F%3Ffbclid%3DIwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME&h=AT2nxOH0KaRlivWMgZMsp0qsKd8Fj1YcMDXiZUf-aGa_d6GghONnlwzm26CP-eEJg_p2IdMhZNCtnxN0Cg2vlZyT003EXzGpZrOcUEARyItpjvICPHhsU4AvxO4), [Pantheon](https://pantheon.io/?fbclid=IwAR08lbNKBfmhPXvcjWH1LVyNWHRmNwBWW7EYww_qHmifWnVI9wAkqfYcqME) are some of them.

Like the Managed VPS, this will be a closely controlled environment that you might not get full access to. That’s ok because you pay the big bucks to get them to manage it for you. Together with the hosting company, you will define the number of users, database space and file space needed, they will take care of the rest. The only thing to check with these companies is the opening hours for support.

If your not in their timezone, then you will have problems getting the support you need as they will be closed.

#### Pro's

* Outrageously fast.
* Closely controlled stack.
* Strong Security.
* CDN often included in the deal.
* Some can do more than just WordPress.

#### Con's

* Maybe no SSH / WP-CLI.
* Support hours may not be 24/7.
* Packages limit the number of websites that can be run.
* Account management might only be on US hours.

#### Target Audience

This is the realm of the large business, a digital agency that resells sites or enterprise client. It's a comprehensive solution backed by high-quality support.

#### Recommendation

If you can, this is the area to aim for if you already have a successful online presence. However, before you buy, properly understand your current usage so that you can buy the right deal for you. If you have a website in your portfolio that has only a few pages and few visitors, this is a very expensive solution for it, consider putting small sites on other hosting and keeping this for the premium sites.

## Dedicated Server Hosting

You have a whole server to yourself when you buy a dedicated server. There are 2 options here, managed or unmanaged. Unless you have a really specific reason, choose managed.

If you choose this route, really consider a good conversation with the hosting company to define the type of machine you need or risk quick upgrades and unexpected costs. It is argued by many that the need for a totally dedicated server is not needed with server virtualization techniques, and a dedicated virtual server is just as good.

#### Pro's

* Blistering speed.
* No contention for processing, disk or memory.
* Good support.
* As many websites as you want.

#### Con's

* Might be outside the budget that you want to spend.
* Upgrading to a new server can be costly and time-consuming.

#### Target audience

Businesses that demand fast performance of their hardware. Those that are serious about e-commerce and do not want to run any risk of another site is being on their server. Again the higher tier customer wants this.

#### Recommendations

Like the managed VPS, this hosting company can be anywhere in the world, so consider choosing one that is close by your customer base to reduce the round-trip when getting data. And remember to choose one that has support hours in your prime-time.

## Dedicated Multi-server infrastructure

Once your hosting needs reach real scale than having a dedicated multi-server infrastructure is vital. This means Load Balancers to split the traffic over multiple caching varnish server, which in turn connect to multiple web server, who connect to a matrix of self duplicating database servers with a dedicated write server for your editors and many databases read servers for your site visitors.

Add on top all the Memcached, Redis and shared file storage you can think of, and you have a proper server infrastructure that can handle \*very\* high user numbers. Depending on the hosting company you work with, in this type of situation, it will be the hosting company that will do all the setup and maintenance of the infrastructure, where your DevOps people will need to be clever with the configuration and setup of the application \(WordPress\) you run. Others might just do everything for you. Expect to start around $40k per year for this kind of infrastructure with support.

#### Pro's

* You will be able to have as many websites as you want on here.
* Support will resolve tickets as soon as possible.
* Your resilient system will handle whatever visitors numbers you need.

#### Con's

* You pay by the GB for storage of files and database.
* Each part of your infrastructure will come with a price.
* SLA's \(Service Level Agreements\) can become very pricey.

#### Target Audience

Those that have traffic in the multi-millions of visitors per month, who want great support and a large amount of flexibility.

#### Recommendations

Really consider a hosting company that is close enough to visit. You are running a premium stack and want to be able to knock on their door if you are not getting the support or performance you need.

## Co-location Hosting

Your kit in someone else's computer room. OK, you'll have to buy a rack-based server and be aware that if it has a hardware or software failure you are the one to fix it.

#### Pro's

* You know the kit intimately.
* You're able to upgrade your hardware as needed.

#### Con's

* No server hardware support
* No software support
* You own upgrades and patching

#### Target Audience

Those that used to have a computer room in their building but do not want that responsibility or cost any more.

#### Recommendations

Research highly your choice of location, pay special attention to security, continuity, closeness to your company, and the peering point of the hosting location you choose.

## Redundant Failover Hosting

This could be with physical or virtual servers, that exist to jump into action when needed. The point being that if some catastrophic failure happens \(think floods, power outage, EMP pulse, plane crashes into the building\), then your money-making enterprise needs to continue.

Usually, this is enough distance away from the main installation that it is felt to be safe. Depending on your disaster recovery plan, this could be in the next-door building, 20KM down the road, or on the other side of the world. The main thing is that there will need to be significant connectivity between the two sites to allow for your database and files to be replicated on a regular basis.

#### Pro's

* Your business will not fail if the power cuts out or any other disaster.

#### Con's

* Really very expensive.
* Regular testing of the failover needed.

#### Target Audience

Those that cannot afford even 5 mins without their website online.

#### Recommendations

Weigh up if you need a ‘like for like’ failover or something that can run in a reduced capacity, to reduce your total costs. Also, have a plan for the human aspects of failover. Do your engineers need to fly to where the servers are? Do you need to tell your editors to stop working on the system so that write traffic is reduced? etc etc.



