hey and welcome to video number six of
the beginner course for NN in this video
we'll be covering some useful nodes and
we'll keep on building the uh workflow
that we've been working on for the past
few
videos so diving right in uh let's
quickly cover some uh super useful nodes
um just as a reminder so far we've seen
the Google Sheets node we've seen the if
node we've seen the schedule uh schedule
trigger node um in this video we're
going to be covering some different
nodes that will likely come in handy uh
when building
workflows first of all we have the edit
fields or set node the edit field node
is useful for managing data in your
items um this is going to be for example
cleaning up the data that we're
currently using in the workflow
um but it can also be used to add format
reduce U any data that's in the items um
so that later on in the workflow we can
be working with uh data that's going to
be a lot
cleaner in uh the edit fields we have
the option to keep only um the fields
that we are setting or to include all of
the input fields
another useful node is one of the uh
sort of function nodes in this case
we're going to be talking about the
aggregate node which is part of a
category that helps dealing uh with
multiple items so this one specifically
is used to aggregate data across all
items in this example we see uh we have
two input items each uh person having
their email and we can aggregate across
the email field and take these two items
and turn them into one single output
item that contains all of the emails
from all the input
items we also have a similar nodes to
remove duplicates to limit the total
number of items or for example to break
out one key of an item into multiple
items which is going to be the opposite
operation of the aggregate
node another very useful node this time
in the trigger category is the web hook
node when this trigger node is added to
your canvas you will be attributed a URL
uh here under test URL and production
URL that you can modify if you'd like
when testing this step or once the
workflow is activated it will listen to
either the test URL or the production
URL for incoming web hooks then it will
execute the workflow with the received
data from the web hook as the initial
data so you can automate from that web
hook
reception Let's uh jump into naden and
keep building that workflow we've been
working on and also show you a quick
example with web
hooks so here we are back in nen uh with
the workflow that we've been uh slowly
building upon these past few videos
let's start off by testing the workflow
so we have all of our
data the next thing we're going to want
to do is first of all clean up a little
bit the data that we're working with uh
we're going to be doing uh this with the
edit Fields node this is a very very
useful node um and I always recommend
that you use the edit Fields node to
make sure that you're only keeping uh
the data that is useful to you this will
avoid you from having very complex items
with many uh fields that you might not
be using um anyway you can always go
back to that edit Fields node and add
some extra fields if necessary so here
in the include in output setting we're
going to choose no input Fields because
we're going to map everything that we
need uh directly as Fields here so we're
going to add a full name field that as
we sort of saw before is going to be
first name and last name and we're going
to use the two uppercase function to
make sure that you the last name is per
case so here if we test this step we
will only have the full name for each
item as uh we showed in the last video
we do need the email so we're going to
uh drag the email
field and name it email and we also need
the company field so we're going to drag
the company field and name IT company
again here if we execute this step we're
only going to have the information that
we need full name email and
Company from this step we can all the
future nodes should execute as they were
supposed to previously we again have the
email key that's available here and the
email key that is available that is
available and used here so what we're
going to do is we want to read the data
from this sheet then we're storing and
making some light formatting to make
sure that we only keep the uh keys that
are useful to us and what we're going to
do is we're going to close off this very
simple workflow by sending some slack
messages uh with the items that satisfy
this condition so here from the true
branch
we're going to use the aggregate
node and here make sure we execute the
previous Fields so we can sort of see
what we're what we're dealing
with um and we're going to aggregate
the email field so here we want to
aggregate individual fields and here we
want to input the field name so some
nodes will ask you for field names and
in this case we're not going to use the
Expression so here if you try to drag
and drop the expression it would
automatically give you the name of the
field and not the expression so here I
can test this step and we're going to
see we have one output with all of the
emails that satisfy this
condition here it might be uh
interesting to include uh the
information on their company so I can
also Al add another field to Aggregate
and in this case just type in the
company field so here we have all of the
emails and all of the
companies what we'd like to do from here
is send a slack message with um all of
the emails and all of the companies that
have signed up so we're going to use the
slack node and the send message
action I already here have a slack
account connected um this should already
be set up in your instance and here I
can write a very simple text
message recap of the signups of the week
for example make sure we're using an
expression and then here I can add
in the list of
emails and then company compies that
signed up this
week and I can add the list of
companies here if I test this step oops
I need to decide who I'm sending the
message to so a user and I can just type
in my name right here
Maxim so we using my slack credential
that I've already set up sending a
message to a user which is myself with a
a recap of all of the signups which is a
list of the emails and a recap of all of
the companies which is a list of
companies if I test this step I get a
slack notification and here we get the
response from slack saying you know okay
the message was sent here is the message
and I can see that recap of signups this
week I have all the emails companies
that signed up this week I have all the
companies
another way of doing this and because of
nen's um because of the way niden
executes each node once per item we
could have decided to not aggregate the
data so here if I delete this node it'll
just reconnect these
two and
here I could change the message and say
new sign up from email
comma company name colon
company and what this will do as we saw
in the uh previous slides instead of
having only one message with all the
emails and all the
companies here we have five messages so
one message new sign up Marcus at
Quantum then we have the next message
new sign sign up sopia from Horizon etc
etc so because here we only have a few
um people in our items it doesn't really
matter if we send just one message as a
recap or one individual message per
person however if we were to deal with
tens or hundreds of emails we would
always prioritize um aggregating the
data and sending it as one recap
message now let's take a quick look at a
web hook workflow example so here I'm
going to add to my canvas a web hook
trigger this will give me a test URL and
a production URL the test URL will be
used when testing the workflow and the
production URL will be used once the
workflow is activated or has been pushed
to
production so here we're going to use
the post method and I'm going to copy
this test
URL from here I can listen for the test
event and behind the scenes I have a
little script that is going to allow
me to send a little test event so here
we can see uh so I used a little python
script to send the
um uh body to the web hook
uh trigger and here we have the usual
headers parameters query and the body
the body has information on my first
name last name company email domain and
the
event so here now that we've tested the
workflow I can uh start building the
workflow from this data something you
might want to do at this stage is just
pin the data so if you have to leave and
come back you can always keep working
with this data so here if I refresh
refresh the
workflow I still would have access to
the pinned data and then from here we
could build a pretty simple workflow
if the
event is equal
to invited team member
then we
send a slack
message that
says
again sending to a user sending to
myself and I can send a simple message
for
example email just invited a team
member and then I can test this step and
then every time a new team member is
invited I will receive the web hook
it'll go down
through and then it'll send me the slack
message if I had um so here I might have
different kinds of events so I might
have a um instead of a invited team
member I might have an account created
event so what I'm going to do is unpin
the data and send another test
payload this time
I'm going to send a payload
with an account
created event so this time not invited
team member but account created and I
could create a another Branch for
example
if the event is equal
to account created
then I want to
send a slack
message very similar to
myself with a message
saying
email just created an account
now I just need to clean up the
connections
so here I want it here it's if it is not
the account created then I check if it's
the team member invited and here I have
a very simple workflow that will let me
know depending on the kind of event we
could obviously switch out the if nodes
for switches um but that's for next time
thanks for listening to this sixth video
of the end and end beginner course where
we kept on building at first workflow
and had our first little play around
with the web hook node something that's
interesting to note here is more than
just uh filtering or creating different
branches we dealt with multiple items
and that's going to be something that's
going to uh be very useful when building
your workflows if we need to well
aggregate some average data across a set
of items in the next video we'll be
covering how nidn stores past executions
of workflows and how we can use that to
better handle uh errors so see you in
the next video