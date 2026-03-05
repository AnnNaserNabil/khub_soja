In this video, I'm going to show you how
you can master nit just by learning 17
notes. And these are the exact same ones
that we use pretty much 80% of the time
when building automations for clients.
And honestly, these are probably the
only ones that you'll ever need. And in
case we haven't met, my name is Mikuel
and over the past 12 months, I've helped
over 40 businesses implement AI and
automations and taught over 17,000
people in the process, all starting with
zero technical knowledge. So if you're
looking to fast track your learning with
any 10 without having to go through a
100 tutorials, well let's dive in. So
Triggers
the first one is the trigger. Now the
trigger is essentially the first step of
the automation. Like what is that thing
that starts the automation for us to
then do the next steps? In any we have
different types of triggers. Uh the
first one is a manual trigger. So we can
just execute the workflow manually. Now
this right here is something that we use
most of the times just for testing,
right? Because we want to test
something. So that has to be a first
step and that's where we add a manual
trigger. Now the second one is a
schedule trigger. So this trigger right
here is primarily used when you want to
run the automation let's say once a
week, once a month or at certain
interval of time. So if I go in here, I
can see that I can run it um in seconds,
minutes, hours, days, weeks, months, and
even custom. So this is cron, which you
can put your maybe every 2 p.m. on a
Tuesday or every 3 p.m. on a Friday of
every month. and you have different
settings that you can put so that you're
able to um to run the automation at a
very specific time of the month of the
year of the week of the day. Right? So
to give you a use case, let's say we had
a content system. Let's say we wanted to
generate content automatically every
single day. What we would do here is we
would go to trigger interval which is
days 1 midnight and triggered min zero.
And this is where we put this as the
first step of the automation so that it
runs every one day at midnight of the
week. Right? Right? So, we're able to
run the automation without us having to
manually trigger it. And this is for the
time trigger. And then we have the type
form trigger. Now, this is just to show
you an example of a trigger that is not
native to any. These two are native to
N10, which means that N10 owns those
nodes. But in this case, type form is
not any. It's a different platform. And
so, Typeform itself is a platform that
generates forms. So, we can generate
forms. And this trigger right here
starts whenever someone fills out the
hiring form in this case. So I have the
form pulled out here which is a form
that is connected to this type from
trigger and what happens is that
whenever someone fills out the form this
sends a signal to this node because it's
an onapp trigger. Uh that will allow us
to then be notified and start the
automation. So I'm going to press exit
workflow. As you can see now this is
waiting for you to create an event in
type form which is waiting for me to
fill out the form. I'm going to press
submit and as you can see here this is
now triggered and we got the data the
name the email the phone number and
location as well. Right? So this
essentially is an onapp trigger which
means that it triggers whenever an
single app starts something or whenever
something happens within an app that we
can all find right here. Add another
trigger. We have all these triggers and
the on app event is the one that you can
use for whatever it is. So these are all
different softwares that can be the
first step of the automation that
triggers whenever something happens
within that software. Now the next part
Storage Solutions
of the nodes are going to be storage
solutions. Now, storage solutions is
just a fancy way to say we use nodes
that allow us to store information in
them, right? And so, typically we use
Google Sheets, we can use Air Table, we
can use Notion, we can use Enit's new
native data tables, which only came out
a few days ago in order to store
information. This could be a simple
Google sheet that looks like this, which
has full name, email address, location,
and phone, but could also mean a data
table within Nitn, which has name,
email, phone number, and location, which
is the exact same. The only difference
is that one is stored in Nitn and the
other one is stored in Google Sheets. So
let me show you exactly what I mean
here. Let me put the um manual execution
here and I can see that inside I am
adding a row. So append row means I'm
adding a row in the N10 17 nodes which
is the name of the Google sheet right
here and I'm putting the first name or
the full name as Mikatory the email is
this and the location is this. I can
press execute workflow which will now
run the step right here and it will add
the row into the Google sheet which will
be mik email and uh location as well and
I can also add the phone which you can
also add by refreshing and let me add
let's do 1 555 1 2 3 4 5 6 7 and if I
execute the step again I can now see
that I will have a new row that will be
added here with email location and this
as well. Now the reason why it gave us
an error is because Google sheet doesn't
like when you put plus signs or equal
signs and so on because these are native
uh equations within Google Sheets. Uh
but you get the gist here. You just
store information into Google sheet. The
second one is within the niten data
tables. So let me connect this. And in
this case we do the exact same thing. It
will be new people because that is the
name of the database. And we're adding
the first name or the name as mik
authority my email my phone number and
location as well. And if I press execute
step, I can see that now it was
successful. And if I go here and I
refresh, I can now see that the data was
added here. Full name, email, phone
number, and location as well. Now, the
amazing thing is that once you master
Google Sheets and Nit's native data
tables, you can pretty much use Air
Table, you can use Notion, you can use
Asana, ClickUp, all these places where
you store information, they're all very,
very similar, right? So, if you
understand one, the setup will be very,
Universal Data Processing
very similar for the other ones. All
right? So now we get to the universal
data processing. So here is where we
manipulate data, right? Manipulate data
just means that we take some sort of
data that comes in and then we structure
it in a way where it makes sense for us
to structure it in. Now in this case I
want to take the um manual execution
here. I'm going to attach it here.
And I can see that inside the edit node
if I execute the step I can see that I
have an array which is basically a list
of people's uh information. So we have
name, email, phone number and location.
David Smith, David Smith, Emily Chan,
Carlos Ramirez, and Asia Khan. And these
are basically giving me the different
pieces of information. Now, the only
problem with this is that let's say we
want to add this to our Google sheet. We
can't because this is an array, which
means that it's inside this right here.
So, this is where the split out comes
in. So, what this means is that if you
want to process each name individually,
right? If you want to process this right
here individually, then this right here,
then this right here, and this right
here, and this right here as well, then
we have to split them out. We have to
take them, as you can see by the diagram
here, it's taking them, which is a
singular thing, and then it's splitting
them out based on the amount of items
that are in the array. So, let me just
show you. Let me just run this right
here. And let me show you that now we
have five items. So, as you can see
here, it went from one item, which is
the array, to five items. And so what
we're doing here is we're processing
each person's details individually
through the automation in the next
steps. And this is splitting out. So
this is when you have an array and you
want to split out each item of that
array. Now aggregate right here is the
exact opposite. As you can see the
diagrams are opposites. And so what we
do here is just bring it back to the way
it was before. Right? And if I press
execute step I can see that now I have
the array that is back there which is
one item. So you can see we have one
item, five items and one item as well.
Now, typically you would use split out
whenever you have because a lot of the
times we get data all within the same
array. We get a bunch of details,
contacts and so on all within one
singular item. And so what we want to do
is go inside that item and take them out
of each one and process them
individually in case you want to add
them individually to let's say a native
data table or even a Google sheet,
right? You have to process each one
individually for it to actually make
sense to add them to a database or
whatever it is that you want to do.
Whilst this node right here basically
when we have a lot of information that
is a lot of items and we want to put
them all together smash them all
together to then send them through to
the next steps right so it's aggregating
data so it's not everywhere it's not in
different places it's all within one
place then we get on to the logic which
is if so this is saying if this equals
to this then it's true if not it's false
so if I say in this case let me just uh
take this out uh there we go right here
so let me say if the name contains
um contains Sarah
which should only be one name then we
send it through the true route right so
if I go here I can now see that only one
went through here and the other ones
went through here now this is great
because automation again is logic so if
you want to send the automation in
different ways based on whether
something is true or false then you can
use the if node right here now the
switch node is a bit different because
it gives you more flexibility right let
me show you exactly what I mean. Let me
say right here inside I have options to
basically route the automation. So I can
say if the name equals to Sarah Johnson
if the name equals David Smith then we
can rename the output to Sarah rename
the output to David. And what this will
do is that it will run through every
single one and the ones that are Sarah
or Sarah Smith in this case. Sarah
Smith, Sarah Johnson, it will send them
through Sarah and if it's David Smith,
then it will send it through David. So
if I run this, I can see that we only
have one item and one item here because
those are the only ones that applied to
that logic. And so how is this different
from if? Well, if only gives you the
opportunity to add conditions, but don't
add more than true or false options.
Whilst here you can put as many options
as you want. And this is great because
you could have a use case where you have
emails. You categorize the emails based
on whether they're FAQ, whether they are
uh promotional, whether they are just
normal, whether they are something else.
And an AI categorizes it. And then you
can use a switch node to then say if the
email is FAQ, then you send it this way.
If it's something else, then you send it
this way. If it's something else as
well, because you can keep adding root
rules. You can say if this equals to
whatever just hello you can rename it to
FAQ and this will be another option
right and you can do as many options as
you want which is amazing because that
allows us to be able to route the
automation based on the type of input or
the output that we get in the previous
step. All right then we have the code
node. Now the code node is amazing uh
because it allows us to be able to do a
lot of things at once and code is one of
the things that again a lot of no code
platforms don't have because in the name
no code uh but the fact that Niten added
the code option here made it so much
easier for us to be able to to transform
information from unstructured to
structured in the easiest way possible
and it's very very fast. Yeah. So let's
say we have the array here which is the
exact same as we had before which are
just a bunch of names that are all uh
within the same array and we can process
each one individually. Now what we can
do with a code node using code and by
the way if you're asking how did you
write the code I just said split out the
items from the array and the array is
this and then if I run this I can see
that now really quick it uh split out
the items in five different um
separately. So now we can go through
five items individually. Now this did
the same function as this. So this
wouldn't make a lot of sense, but if you
have something a lot more complex and
you need the data to be transformed in a
way where it would take you seven or
eight different nodes, then you can do
it all within the code node. Now, I'm
not going to get into too detail about
this because again, we're all here to
not write code, right? Uh but just know
that you have this option to be able to
write code with any and it makes it a
lot easier to do more complex stuff. You
won't really need them for most of the
time, but just knowing that you have the
option there to use gives us a lot more
flexibility when we're building
automations. And then we have the merge
node. So let me actually set this up.
All right. So for the merge node, what I
did is I put two different fields. One
is hello yo and then the other one is
hello ciao. And when I run this
automation, it will run this one first
and then it will run this one. And let's
say I put a wait node here. I can put
two two seconds just for it to wait 2
seconds. I can show you that this right
here will run first and then it will run
this one. Right? Now let's say that we
had an automation where we had to run
this and then we wanted to run this and
then we wanted to go right without us
having to go through here first and run
the whole automation and then run
through here first. Now let's say that
we had an automation that we start here
and it goes two different ways but then
we want to aggregate the data. We want
to send the data in the same way as two
different inputs input one and input two
to then be able to take both pieces of
information to then be able to go to the
next steps. So that's exactly what the
merge node does. If I go here, I can see
also I can add more inputs. So if I
press four, it basically uh it can it
can intake more inputs at the same time.
In this case, I can do two. I can
execute the step. And now we can see
that we got two items, one is yo and one
is cha. So what it's doing here, it's
it's aggregating both of them together.
Now there are other options as well um
to combine to then SQL query. So we can
query different things or different data
points and then we can also choose a
branch. Now honestly the one we're
mostly going to use is going to be
combine and it's going to be append. Now
append in this case will be output each
item individually and combine is taking
these two and just putting them all in
the same sort of array that you can use
for the next steps. Now here's a system
that I built that uses the merge nodes
three different ones. Uh and the way
that this works is that we have um the
length post which is made the Facebook
post which was made and then we want to
merge them. So we have the Facebook post
and the LinkedIn post and then we have
the Twitter post which is made, the
article which is made and then we're
merging these two together as well to
then finally merge all four things
together. So we can directly send them
to the Google sheet. And the reason why
we're merging them is because if we run
each one individually then we would have
to set up a new automation for each row
at a single time to add them to the
Google sheet which we can actually just
remove by just merging them all together
in the same sort of structured way as
different items to then send them to the
Google sheet. So the next nodes right
Connectivity & APIs
here are going to be connectivity and
API. Now API is the most important skill
when it comes to automation. And if you
haven't watched my video about
fundamentals of API, just watch it up
here. U but it is the reason why apps
get to talk to other apps, right? It's
the reason why this app right here can
talk to an intent, but this app right
here can talk to this one, right? Or we
can just talk to each other um in
different ways. So in this case, the
first node that we have to learn is the
HTTP request. Now this is one of the
most powerful nodes in any 10en because
when we go here and I go to action app.
So this is taking an action within a
single app. We can see that we don't
have an infinite amount of nodes right
like we can stop here and then what what
if we have a software that we want to
use but we can't find it here. What do
we do then? Well in this case we use the
HTTP request. So let's say I wanted to
use the free weather API which gave me
the API of the weather. And if I go here
to end, I can't see any app that's
called free weather API. So what I have
to do is I have to set up the HTTP
request of the free weather API, which I
can find in the documentation right
here. By the way, I covered this in the
API fundamentals video. So if this makes
no sense to you, please go watch that
video cuz it will help a lot. Um, but
this right here is the API
documentation, which is essentially a
documentation which allows you to see
exactly what you can automate and what
you cannot automate within a software
and also how to set up the automation.
And so by looking at this um
documentation here, I was able to set up
the um the automation of the weather API
which looks at the temperature in London
right now. And if I execute the step, I
can see that now I'm still able to
actually run the automation. I was still
able to use the software without having
to use these softares here which are
already pre-made by 10. And you can see
here we have the different outputs. So
London, we have the temperature, degrees
in Celsius, Fahrenheit, and different
other informations as well. Then we have
web hooks right here. Now, web hooks are
a way for us to get notified when
something happens. So, let's say someone
fills out a form. Well, it sends the
data to the web hook. You remember this
right here, the type form trigger. Well,
behind the type form trigger is actually
a web hook which allows us to be able to
get notified when something happens.
Well, in that case, it's whenever the
form is being submitted, it sends the
data to the web hook through an API.
Right? So, right here we have the test
URL which is used for testing and
production URL which is used when the
automation is set to active. And then we
have the different type of HTTP method
which again is very similar to there
because we have post we have all these
options and these are the same ones that
we have here. And then we have the path
which is just the way that we uh that we
name the URL. And then this right here
is a thing that you would connect to the
software itself to then be able for us
to get notified when something happens
within that software. And respond to web
hook will be the thing that will then
respond back to the server that we got
the information from. Now, to show you
exactly how it works, what I'm going to
do is use a software called Postman API,
which allows us to send example uh test
data to the web hook, right? I'm going
to go in here, copy this, make sure you
change this to post request, and then
I'm going to paste this here. Make sure
this is the post request as well. And
then the body will be what is the thing
that we're sending the web hook. In this
case, let's just do full name,
my name. Let me run this here. I want to
press send. This will now start the
workflow. As you can see here, now we
started the workflow and we sent the
full name here which is the body, right?
And this is how we send information from
one server to another through a web
hook. Now let's say I had this
connected. Now all I have to do here is
I have to change this to using respond
to web hook node.
And here um what we can do is
we can just respond with workflow has
finished.
And so when I run this,
I send this here. I can see that now we
get the data workflow has finished. Now
this is amazing when we are sending data
to a software or to a web hook in this
case and we want to get something back.
So let's say we send the signup link to
a user in some sort of software. He
signs up, we get something back. Lastly,
AI Integration
it would be the AI integration. So this
is one of the ones that are most uh
commonly used. Um first one is the AI
node. So this is just to create
something with AI. So in this case we
can say create a LinkedIn post
about life and here we have our openi
connected the resource is text the
operation which is the action that we're
taking is good uh the model it can be um
you have a list of models here you can
use GPT4 or latest or you can use
whatever you want and then you have the
prompts right here the prompts there are
three different prompts that you can
write the first one is a system prompt
the system prompt is a prompt that you
tell the AI uh you give it an identity
so you are a helpful intelligent XY YZ
assistant. Then you have user. So user
is when you tell it to do something. So
your task is to do XYZ. And assistant is
when you give it some examples. In this
case, let's keep it simple. I'm going to
press execute step. And this will now
create a LinkedIn post about life. And
as you can see, sure, here's a
thoughtful and professional LinkedIn
post about life. And then it gives me a
whole LinkedIn post that we can then
use. Now this AI step is used whenever
we want to have some sort of data or
some sort of input sent to AI just like
we would on chat GBT. automatically and
then give us the output for content for
any structured data that you want
structured for anything pretty much that
you can think of AI playing a role in
the automation just as a linear thing
for it to do just that task in specific
only that task and then we have the AI
agent if you haven't watched my AI agent
101 video make sure to watch it up here
uh it explains exactly how you can build
your first AI agent from scratch using
N10 but the AI agent is the reason
probably the reason why uh N10 blew up
so fast is because it had now the um
opportunity for us to used something
that was connected to AI that could
think through or that could remember the
different conversation we had but that
was also uh hooked up to different
tools. So in this case if you scroll
down we can see that we hook it up to
Gmail, we can hook it up to Air Table,
we can hook it up to well pretty much
anything. Uh we have all these different
softwares that we can use to then be
able to connect it to the AI agent for
it to actually take action. And a good
thing is that the Gmail tool can be
connected and the air table tool can be
connected and the let's say notion tool
can be connected and you can have as
many connections as you want and this
acts as a personal assistant that allow
us to be able to take action on
different things based on the input that
we give it. So in this case what it
would look like is we would have a
trigger which would be usually would be
an on chat message which means that
we're chatting with the AI agent itself.
And if I open chat, I can now see that I
can speak to the actual AI agent.
Something that you can't do with the AI
step. So I can say hello. What this will
do is that it will then talk to its AI.
It will remember the conversation and
then bring it back. It's sort of like a
person that we're talking to. And the
good thing is that the person itself has
access to our software which is amazing
because now it can take action on our
behalf based on the input that we give
it. So if we say draft an email, it will
then go to the Gmail tool and then do
its thing. Same thing with air tableable
and same thing with motion. And the use
case that you can think of this is
pretty much anything any task that you
want a singular input data store. So you
basically have let's say one chat
message right where you have one place
and through a series of inputs. So
through a series of send an email Gmail
send an air table or create a notion
task or whatever it is it takes actions
in different softwares without us having
to create a new automation for every
single action that we want to take. All
right. And if you want the full
Outro
blueprint to this end so you can always
come back to it as a resource. Make sure
to check out the first link down below
which will take you to my free school
community right here. You can go to the
classroom section. You can go to the
templates vault and you will see the
latest video which will be in this case
uh 17 or 18 nodes to master 10. You can
then press this button right here to
download the Nitan automation blueprint
and import it into your own account. And
if you have no clue how to do that, no
worries at all. You can always watch the
tutorial right here which will show you
step by step how to do it. And if you
apply and you get in, you also get
access to the AI automations 101 course,
which is a very, very comprehensive
guide that takes you from a real
beginner in AI automation to someone
who's actually able and willing to uh
build automations for themselves, for
other businesses. The only catch is not
everybody gets in. So, please put some
thoughts into your answers when you
apply. So, that marks the end of the
video. And if you're someone who's been
building with Nitn and now actually want
to go out and sell to businesses and
apply this to a business use case, then
make sure to check out this video on the
screen where I show you 10 proven
business use cases where you can apply
AI agents and AI workflows within an end
within sales, marketing, product, and
service delivery. With that being said,
I hope you found value from this video
and I'll see you in the next