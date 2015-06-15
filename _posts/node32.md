If you already [followed this
article](http://anl4u.com/blog/create-route-53-record-for-load-balancer-on-amazon/) and
created a Route 53 record for your sub domain, that means we can proceed
to add TXT records now. Please note that i assume that you use three
different services for all this like Amazon for hosting,
Godaddy/Name/NameCheap etc for domain, MailJet/SendGrid/ElasticEmail etc
for emails send out.

Now for example, i want to send my users a welcome email when they join
my blog [[no-reply@blog.anl4u.com](mailto:no-reply@blog.anl4u.com)]. As
you can see blog.anl4u.com. Now if you try to insert a SPF record for
blog sub domain in TXT section of your hosted domain DNS area. Most
probably it will throw an error, saying that there is already an entry
for blog in CNAME section. Considering that you are using load balancer
and added blog sub domain in the CNAME section for that load balancer.

So to overcome this issue, simply create a Route 53 record for your sub
domain and attach your load balancer to that sub domain. Follow [this
article](http://anl4u.com/blog/create-route-53-record-for-load-balancer-on-amazon/) to
create a Route 53 record.

Let’s add SPF record 1st, click on the Create Record Set button. Leave
the name field as it is(it will show your sub domain). From the Type
select TXT, in the Value field paste the text you get from the Email
provider for SPF with quotes and save the entry.

[![route53-2](http://anl4u.com/blog/wp-content/uploads/2013/03/route53-2-205x300.png)](http://anl4u.com/blog/wp-content/uploads/2013/03/route53-2.png)

Create a DKIM entry now. In the Name field add this ‘mailjet\_domainkey’
without quotes. As you can see, i am using mailjet. You can change that
to yours. Choose TXT from the Type drop down. Input the long line in the
value field with quotes and save it. Now you will have 5 records NS,
SOA, A, 2 TXT.

[![route53-1](http://anl4u.com/blog/wp-content/uploads/2013/03/route53-1-300x139.png)](http://anl4u.com/blog/wp-content/uploads/2013/03/route53-1.png)

I assume you already added the NS records for your sub domain by
following the article mentioned above. Now go to your Email sender
service and validate the SPF and DKIM entries.

[![route53-4](http://anl4u.com/blog/wp-content/uploads/2013/03/route53-4-300x202.png)](http://anl4u.com/blog/wp-content/uploads/2013/03/route53-4.png)

And that’s it, happy sending emails.

**Reference:[http://anl4u.com/blog/how-to-add-txtspf-dkim-records-for-sub-domain-in-r...](http://anl4u.com/blog/how-to-add-txtspf-dkim-records-for-sub-domain-in-route-53-on-amazon/)**
