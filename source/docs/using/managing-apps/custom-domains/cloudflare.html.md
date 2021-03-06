---
title: Configure SSL-Enabled Custom Domain
---


This page has has instructions for setting up an SSL-enabled custom domain using CloudFlare, a content distribution network. This is the Pivotal-recommended strategy for using SSL with a custom domain. In the resulting configuration, traffic to and from your application running on Cloud Foundry will be routed via CloudFlare.

browser  < --- ssl --- >  CloudFlare proxy  < --- ssl --- >  Cloud Foundry

To enable this, you will configure CloudFlare’s “Full SSL” option for the domain. You can use a CloudFlare-generated SSL certificate between the browser and the CloudFlare server, or if you prefer, configure CloudFlare to use a certificate you provide. A Cloud Foundry-generated SSL certificate will be used between the CloudFlare proxy and the application on Cloud Foundry.

These instructions assume that your domain is already registered with a DNS registrar. To complete this procedure, you must be able to access the DNS registrar for the domain. Note that you do not need to change DNS registrars. The only change you will make with your registrar is to point the authoritative name servers to CloudFlare's name servers.  

It is recommended that you download the zone file for the domain from your DNS provider, for use in [Step 5 - Configure DNS Records](#configure).

## <a id='sign-up'></a>Step 1 - Get CloudFlare Account ##

Go to https://www.cloudflare.com/, and click **Sign up** in the upper right of the page. On the registration page, enter your email address, username, and password. Accept CloudFlare's terms of use, and click **Create account now**. 

![Register](/images/cloudflare-register.png)


## <a id='add-site'></a>Step 2 - Add Domain to CloudFlare ##

On the CloudFlare "Add a website" page, enter the name of your custom domain and click **Add website**. CloudFlare will query authoritative DNS servers for the DNS records registered for the domain. 

![Add a Web Site](/images/add-website.png)


## <a id='scan'></a>Step 3 - Wait for Scan to Complete ##

When the scan is complete, a **Domain records scanned** callout appears. Click **Continue Setup**.

![Scanning](/images/scan-complete.png)

## <a id='configure'></a>Step 4 - Configure DNS Records ##

The CloudFlare "Configure Your DNS Records" lists the records it obtained for the domain. In the row for your domain name -- "uberconnected.com" in the example --- the cloud icon in the "Active" column should be orange.

To ensure your DNS records are correct and complete, Pivotal recommends you download the zone file for the domain from your DNS provider, and then upload it to CloudFlare --- click the **Upload a zone file** link to do so. 

If you prefer, you can click **Add** to define additional records, if necessary. 

When done, click **I've added all missing records, continue**. 

![Add a Web Site](/images/config-dns.png)

## <a id='settings'></a>Step 5 - Choose Settings ##

On the "Settings for \<YourDomain\>" page:

*  **Choose a plan** --- Select "Pro" or "Business". Do not choose the free plan; you can only configure SSL for a domain if you have a paid CloudFlare plan. If you choose "Pro", CloudFlare will generate an SSL certificate for communications between browsers and the CloudFlare proxy. If you prefer to provide your own SSL certificate, choose "Business".
*  **Performance** --- This configures a performance profile that sets the values of multiple performance-related options. You can choose any of the available options, and easily change it later. After completing this configuration process, follow the instructions in [Reviewing CloudFlare Configuration Options](#review) to understand the performance (and also security) options, and tailor your configuration, as desired. 
    * **CDN only (safest)** --- This is the default.  
    * **CDN + Basic Optimizations** 
    * **CDN + Full Optimizations**
* **Security** --- This setting controls CloudFlare's which visitors are shown a captcha/challenge page. The options are:
    * **I'm under attack!** --- Should only be used when a site is having a DDoS attack. When this setting is configured, CloudFlare will present an interstitial page to each site visitor (for about five seconds) while validating that the request came from a human, rather than a web robot. 
    * **High** --- CloudFlare will challenge all visitors that have exhibited threatening behavior within the last 14 days.
    * **Medium**
    * **Low** --- CloudFlare will challenge only the most threatening visitors. CloudFlare recommends this as initial setting.
    * **Essentially off** --- CloudFlare will act only against the most grievous offenders. 
*  **Automatiac IPV6** --- The options are: 
    * **Full IPv6 Gateway** --- Enables IPv6 on all subdomains that are CloudFlare-enabled (marked by an orange cloud in your DNS settings)
    * **Safe IPv6 Gateway** Will only create an IPv6-specific subdomain for your site (www.ipv6.yoursite.com).
*  **SmartErrors** --- SmartErrors is a CloudFlare feature that prevents 404 errors from being presented when a site visitor requests a page that is not found. Instead, CloudFlare performs a site search based on the user's request, and presents a list of best-matching pages from the site. 

Click **Continue**.

![Add a Web Site](/images/settings.png)

## <a id='ssl'></a>Step 6 - Confirm SSL ##

On the "Confirm SSL" page, you can select:

* **Automatic SSL Configuration** --- If you select this option, CloudFlare will generate an SSL certificate. If you selected the "Business" plan in the previous step, you can upload your own certificate later.
* **Custom SSL Configuration** --- This option only appears if you selected a "Business" plan in the previous step. When you select this option, you can upload your own private key and certificate files.
* **Manual SSL Configuration** 

![Add a Web Site](/images/confirm-ssl.png)

## <a id='settings'></a>Step 7 - Update Name Servers ##

The "Update Name Servers" page lists your current name servers, and the CloudFlare name servers with which to replace them.

Log on to your domain name registrar and open that page for name server management. Replace the currently configured name servers with the CloudFlare name servers.

Click **I've updated my nameservers, continue** on the "update your name servers page."

## <a id='settings'></a>Step 9 - Setup Complete ##

When you have provided all necessary information, the "My Websites" page appears, and states that you have completed the domain setup.  

Check your DNS registrar's site to verify that the name server changes made in the previous step are complete

![Add a Web Site](/images/complete.png)


## <a id='review'></a>Review CloudFlare Configuration Options ##

After your initial domain setup is complete, you may want to review CloudFlare configuration options. To do so, click the **Websites** link at the top of any CloudFlare.com page to view the "My WebSites" page.

Click the gear icon in the row for your domain, and choose **CloudFlare settings** from the menu. The **CloudFlare settings** page has several tabs that describe CloudFlare configuration options. In particular, see the **Security** and **Performance** tabs to understand more about the options you configured, and alternative settings.

![Add a Web Site](/images/my-websites.png)
















