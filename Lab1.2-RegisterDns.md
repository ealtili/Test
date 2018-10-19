
[Source](https://medium.com/@kcabading/getting-a-free-domain-for-your-ec2-instance-3ac2955b0a2f "Permalink to Getting a FREE domain for your EC2 Instance – Kristian Cabading – Medium")

# Getting a FREE domain for your EC2 Instance – Kristian Cabading – Medium


# Getting a FREE domain for your EC2 Instance

![][7]

I've been studying AWS recently and was looking for an easier way or rather using a domain name instead of a random IP address to access my deployed applications on a EC2 instance. This is mainly for testing purposes and ultimately showing it to a client without purchasing anything. Of course buying a domain name would be the permanent solution but I've found a better way to achieve this without even spending a single penny.(I'm using AWS free-tier by the way)

[Freenom][8] saves the day as it allows you to search for a domain name absolutely for free. The only caveat of using their free domains are the extensions since you'll be getting unusual ones like .tk, .ml, .cf, .ga and .gq. I guess this won't hurt that much as you'll still be able to get the domain name you want for free. Below are the steps on how to get your free domain

* * *

### **Get your Free Domain**

1. Go to [freenom.com][9] and register for an account.
2. Go to Services -> Register a new domain.
3. Enter the desired domain name and check it's availability.
4. Continue with the checkout process and you should be on the same step as the screenshot below.

![][10]

5\. Skip the DNS for now (We will configure it later once we have the Nameservers of our EC2 instance) and under the Period column, select an option for how long would you use the domain. (Maximum of 1 Year to avail it for free)

* * *

### Point your EC2 instance to your new domain

1. Log into your AWS account and assign an elastic IP to your instance if you haven't done so.
2. Go to Route53 and create a hosted zone for your domain and set the type to Public Hosted Zone
3. Once created, you'll be presented with 2 default record sets for your domain. In here, take note all of the 4 Nameservers.
4. Create another record sets for your domain, one with www name and one without it. All pointing to your Elastic IP you set to your EC2 instance. See screenshot below.

![][11]

5\. Once you've have setup the 2 additional record sets, the last step is to just point your domain to use the Nameservers provided by Route53 service.

Finally, login to your [Freenom][12] account go to Services->My Domains. From here, choose the domain and click on Manage Domain. Then go to, Management Tools, then Nameservers. From here, select custom nameservers and just enter all the 4 nameservers and hit on the save button. Just give it a couple of minutes and you would now be able to access your app in your EC2 instance with the domain you choose.


[7]: https://cdn-images-1.medium.com/max/1600/1*GSbFl4tTh60YHWRm89J9rw.png
[8]: http://www.freenom.com/
[9]: http://www.freenom.com
[10]: https://cdn-images-1.medium.com/max/1600/1*mk-DtmeOOwzDZRx7JU0bYg.png
[11]: https://cdn-images-1.medium.com/max/1600/1*9Q3YIpBH1i-aAjcknElFJQ.png
[12]: https://my.freenom.com/clientarea.php?action=domains
