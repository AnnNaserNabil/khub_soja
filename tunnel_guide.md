Chapter 6: Method 2: Permanent Setup (Fixed Domains)
4:284 minutes, 28 secondsThat doesn't change. If we want a stable setup like that, we must have our own domain and that domain has to be active
4:354 minutes, 35 secondson Cloudflare. Now we are moving to that permanent setup. One more thing to be clear about. If you don't have a Cloudflare account yet, create one
Chapter 7: Moving Domain DNS Management to Cloudflare
4:444 minutes, 44 secondsfirst. It's a simple process. Sign up with email and password. Cloudflare sends a verification email and once you verify it the account becomes active.
4:524 minutes, 52 secondsRemember without email verification you won't get tunnel access. I'm not showing the account creation process in this video because it's very easy and our
5:005 minutesmain focus here is tunnel and domain setup. Now for a permanent Cloudflare tunnel setup doesn't really matter where
5:075 minutes, 7 secondsyou bought your domain. You can buy it from Gored, Namechip, Hostinger or anywhere else. But DNS management has to
5:155 minutes, 15 secondsbe moved to Cloudflare. Cloudflare tunnel works directly with DNS and that's why the domain must be active on Cloudflare. In this video, I'm using my
5:245 minutes, 24 secondsown domain rafi udraj.me which I bought from godaddy. Now I will bring this domain's DNS management into Cloudflare.
5:325 minutes, 32 secondsI first go to the Cloudflare website and log to my account with email and password. After login, I am redirected
5:395 minutes, 39 secondsto the Cloudflare dashboard. Now from the left menu, I click the domains option. Inside the domain section, I see an option called onboard a domain and I
5:485 minutes, 48 secondsclick it. Now Cloudflare asks for the domain name. I enter Rafi Udaraj.me and below you can see quick scan for DNS
5:565 minutes, 56 secondsrecords. I leave this option on. Its job is to scan for existing DNS records like subdomains, mail service records or
6:046 minutes, 4 secondsother service records and try to import them automatically. This helps avoid breaking your existing setup during migration. Even if you're using a brand
6:126 minutes, 12 secondsnew domain, leaving this on is fine. Now I click the continue button. Next,
6:186 minutes, 18 secondsCloudflare asks me to select a plan. I choose the free plan because the free plan is enough for Cloudflare tunnel. We don't need any paid features in this
6:266 minutes, 26 secondsvideo. Then Cloudflare redirects me to the next page where I can review DNS records. This is sometimes called the manual DNS entry or DNS review page.
6:366 minutes, 36 secondsHere Cloudflare tries to show any existing DNS records it found during the quick scan. If you already had subdomains, mail services or other
6:446 minutes, 44 secondsservices, those DNS records will show up here. In this video, I'm not editing these records or working with them because our main goal is setting up a
6:526 minutes, 52 secondsCloudflare tunnel. So I don't change anything on this page. I scroll down and I click continue to activation. After this step, Cloudflare takes me to a page
Chapter 8: Updating Nameservers (GoDaddy Example)
7:007 minutesthat shows instructions for updating name servers. Here, Cloudflare clearly explains that the domain has been added to Cloudflare, but it is not fully
7:097 minutes, 9 secondsactive yet. The domain becomes active only after we go to the place where we bought the domain, meaning our domain provider, and set Cloudflare's name
7:187 minutes, 18 secondsservers. On this page, Cloudflare explains the steps. First, we need to log into your DNS provider or domain register. In my case, the domain is from
7:277 minutes, 27 secondsGodaddy. So, I log into Godaddy. If someone else uses a different provider,
7:327 minutes, 32 secondsthey should log into that provider's account. Then, Cloudflare shows that you need to replace your current name servers with Cloudflare's name servers.
7:397 minutes, 39 secondsFor my domain, two name servers are shown. These two are the most important now because they give Cloudflare control
7:467 minutes, 46 secondsof my domain's DNS. I copy both name servers from here because I will need to paste them into go in the next step.
7:547 minutes, 54 secondsCloudflare also makes it clear that the old name servers like the default ones from your domain provider must be removed. You can't keep old name servers and Cloudflare name servers together.
8:058 minutes, 5 secondsOnly Cloudflare name servers should remain for the domain to activate properly. Now I go to Godaddy and log into my account. After logging in, I go
8:148 minutes, 14 secondsto the domain section and select rafi.me. Emmy. Then I go to the domain DNS section. Here I see a tab called
8:238 minutes, 23 secondsname servers and I click it. Now I click the change name servers button. Code ready shows a few options. Use default name servers or use my own name servers.
8:338 minutes, 33 secondsI select the option I will use my own name servers. Then in the name server one field I paste the first name server
8:408 minutes, 40 secondsfrom Cloudflare. In the name server two field I paste the second Cloudflare name server. After both are entered correctly, I click save and then
8:488 minutes, 48 secondscontinue. After this is done, I go back to Cloudflare dashboard. At the bottom,
8:528 minutes, 52 secondsthere's an option called I updated my name servers and I click it. This means I am letting Cloudflare know that I
8:598 minutes, 59 secondsupdated the name servers at the domain provider. This change can take some time to propagate globally. Sometimes it takes 5 to 10 minutes, sometimes 1 to 2
9:089 minutes, 8 secondshour or even up to 24 hours. This is because of DNS propagation and it's completely normal. I can click check name server now to verify. In my case,
9:189 minutes, 18 secondsthe name server update finished in about 8 minutes. Now when I go to the domain section in Cloudflare, I can see Rafi
9:259 minutes, 25 secondsUdra.me showing as active. That means DNS is fully under Cloudflare's control and we are ready for the next step. Now we will create the tunnel from the CLI.
Chapter 9: Authorizing cloudflared CLI with your account
9:379 minutes, 37 secondsFirst I will authorize Cloudflared with this domain. In the terminal, I type cloudflared tunnel login. Now I run this
9:469 minutes, 46 secondscommand. As soon as it runs, a browser opens. In the browser, I select my Cloudflare account and give permission for the domain Rafi Udaraj.
9:569 minutes, 56 secondsThis permission means Cloudflare on my local machine can now create and manage tunnels under this domain. After
10:0310 minutes, 3 secondspermission is granted, I return to the terminal. Now we will create a permanent tunnel. I run the command cloudflare
Chapter 10: Creating a Permanent Tunnel and Subdomain
10:1010 minutes, 10 secondstunnel create nexj live. Here nextj-live is the tunnel name. You can use any name you like.
10:1910 minutes, 19 secondsAfter running this command, cloudflare creates a new tunnel for us. Once the tunnel is created, the terminal shows a
10:2610 minutes, 26 secondslong unique ID usually in hex like mine starting with 1 ed8140.
10:3510 minutes, 35 secondsThis is the tunnel ID. It's very important because Cloudflare identifies your tunnel by this ID. I copy this ID
10:4210 minutes, 42 secondsbecause we will need it again when writing the config file. Now the tunnel has been created but it's not attached to any domain yet. So in the next step
10:5110 minutes, 51 secondswe will create a subdomain and attach it to this tunnel. I run the command cloudflare tunnel route DNS nextJS-live demo.rafiaraj.me.
11:0311 minutes, 3 secondsThis command creates a subdomain called demo.rafi.
11:0811 minutes, 8 secondsIn Cloudflare DNS and maps that subdomain to our nextjs live channel.
11:1211 minutes, 12 secondsAfter running this command, I can check the DNS section in the Cloudflare dashboard to show that the demo.rafi subdomain has been created. That means
11:2011 minutes, 20 secondsany request to demo.rafi.me will go directly to my local server through this tunnel. Now we will configure the tunnel. The tunnel is
Chapter 11: Configuring config.yml & Credentials File
11:2811 minutes, 28 secondscreated and the subdomain is routed. But Cloudflare still doesn't know which local service to send traffic to through this tunnel. We will write that
11:3611 minutes, 36 secondsinformation in the config file. First I go to the terminal routt and run the ls-
11:4211 minutes, 42 secondsa command. The reason is to show whether a hidden folder named Cloudflare exists. Now I can see that Cloudflare folder.
11:5111 minutes, 51 secondsCloudflare creates this folder automatically and it stores all important tunnel files here. Now I go inside that folder with cd.cloud
12:0012 minutesCloudflare. After entering the folder, I create a new config file with a touch config.l command. Cloudflare tunnel uses
12:0912 minutes, 9 secondsa yiml format config file. So we name it config.yiml.
12:1412 minutes, 14 secondsThen I type code config.yiml and the file opens in VS code. So it's easy to write. Now we will write the contents of
12:2212 minutes, 22 secondsthe config file step by step. First I write tunnel colon and put our tunnel ID next to it. This is the tunnel ID we got
12:3012 minutes, 30 secondswhen we created the tunnel and which I copied. Starting with 1 ed140.
12:3612 minutes, 36 secondsCloudflare uses this ID to know which tunnel this config is for. If you forgot the tunnel ID, you can run cloudflared
12:4412 minutes, 44 secondstunnel list in the terminal to see all tunnels and copy it again. Now we write credentials file colon. This is a very
12:5312 minutes, 53 secondsimportant instruction because without this credentials file, Cloudflare cannot run the tunnel. You can think of it as the tunnel's identity card or permission
13:0213 minutes, 2 secondsfile. It lets my local machine prove to Cloudflare, yes, I am authorized to run this tunnel as the owner or authorized connector. We didn't create these
13:1013 minutes, 10 secondscredentials files separately. When we ran the Cloudflare tunnel create nextJS live command, Cloudflare automatically
13:1813 minutes, 18 secondscreated a JSON file. Cloudflare provides this file. So, Cloudflare can authenticate securely with Cloudflare
13:2513 minutes, 25 secondswhen starting the tunnel ladder. Since I'm inside thecloudflat folder, I run the ls command and you can see the file.
Chapter 12: Finding the Credentials File Absolute Path
13:3213 minutes, 32 secondsThere's a dojson file starting with 1 ed140.
13:3613 minutes, 36 secondsThis file is our tunnel credentials file. Typically, the file name starts with the tunnel ID because it belongs to that specific tunnel. Now, we need to
13:4613 minutes, 46 secondsput the full absolute path of this file next to credentials file colon in our config.yiml file. Just the file name is not enough. The full path is required.
13:5613 minutes, 56 secondsSince I'm on a Mac, I use this command to get the full path, real path and the complete JSON file name. As soon as I
14:0514 minutes, 5 secondsrun it, the full path shows in the terminal. In my case, the path is users/rafi and then inside the JSON folder complete
14:1314 minutes, 13 secondspath. Now I copy this full path and paste it next to credentials file in the config.iml file. If you are on Windows,
14:2214 minutes, 22 secondsyou can use this command in PowerShell.
14:2414 minutes, 24 secondsresolve-path and then the JSON file name. If you are on Linux, the same real path command works just like on Mac. No matter which
14:3314 minutes, 33 secondsoperating system you use, the goal is the same. Get the full path of the credentials file and place it correctly in config.yiml.
14:4114 minutes, 41 secondsBecause when Cloudflare starts the tunnel, it uses this file to authenticate with Cloudflare. What does this mean? When I start the tunnel
14:4914 minutes, 49 secondslater, Cloudflare will read this config.yimml YML file and use this credentials file to authenticate with Cloudflare if the path is wrong or the
14:5814 minutes, 58 secondsfile isn't found the tunnel on current because Cloudflare won't be able to confirm that this machine is authorized to run that tunnel. Next, we write the most important part ingress colon.
Chapter 13: Configuring config.yml & Ingress Rules
15:1115 minutes, 11 secondsIngress basically tells Cloudflare which local service should receive traffic for a given domain. I set hostname colon
15:2015 minutes, 20 secondsdemo.rafi Rafiodaraj.me that means any request to this subdomain will follow this rule under it I set
15:2815 minutes, 28 secondsservice colon and our local host URL that means traffic for this subdomain will be sent to the server running on
15:3515 minutes, 35 secondsport 3000 on my local machine which is our next JS app. Finally I add a fallback rule dash service colon http
15:4415 minutes, 44 secondsstatus col 404. This means if a request comes to a host name we haven't defined, Cloudflare will return a 404 response.
15:5215 minutes, 52 secondsIt's a good practice because it handles unexpected requests properly. Now I save the file. Back in the terminal, I run the tunnel by typing Cloudflare tunnel run next.js-life.
Chapter 14: Running the Permanent Tunnel
16:0416 minutes, 4 secondsAs soon as I run this command, Cloudflare reads the config file,
16:0816 minutes, 8 secondsconnects to Cloudflare, and starts the tunnel. If everything is correct, I open demo.rafi.me am in the browser. You can see the app
16:1716 minutes, 17 secondsrunning on my local server is now publicly accessible through this subdomain. That means Cloudflare tunnel is working correctly and the local
16:2516 minutes, 25 secondsproject has been successfully exposed to the internet. Anyone can access it. Now,
Chapter 15: Important Limitations & Use Cases
16:3016 minutes, 30 secondsso far we have seen the entire setup from start to finish. How to use Cloudflare tunnel to securely make a local app public. We did this without
16:3816 minutes, 38 secondsport forwarding, without router configuration and without exposing the public IP. Yet the local server is
16:4616 minutes, 46 secondsaccessible from the internet. Here is a technical point that is important to understand. Cloudflare tunnel doesn't host your app on Cloudflare servers.
16:5516 minutes, 55 secondsYour app keeps running on your local machine the whole time. Cloudflare only creates a secure outbound connection between your local server and the
17:0317 minutes, 3 secondsinternet. That means if your local machine disconnects from the internet or if your local app server is stopped, the
17:1117 minutes, 11 secondsapp won't be accessible through that domain or subdomain. In that case,
17:1517 minutes, 15 secondsCloudflare has no live endpoint to forward traffic to. Keeping that limitation in mind, Cloudflare tunnel is a very powerful solution. It is especially ideal for client demos,
17:2617 minutes, 26 secondsinternal tools, testing, staging environments, or short-term public access. And the biggest advantage is that you don't have to maintain a
17:3417 minutes, 34 secondsseparate VPS or server. Let me remind you again this whole process is not just for NexJS. Any local server running on any port whether it's React, NextJS,
17:4317 minutes, 43 secondsLaravel, Django or any other framework can be made public the way using Cloudflare tunnel. You just have to make sure which port your app is running on.
17:5217 minutes, 52 secondsOverall, Cloudflare tunnel gives you a practical and modern approach to secure networking where your code stays on your machine but can still be accessed from the in it.