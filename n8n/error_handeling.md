hello and welcome to video number seven
of the beginner course for nen in this
video we'll be covering how past
executions are stored for your
workflows and uh specifically how to
handle workflow errors which is a very
important
skill let's start a little bit by
talking uh about how executions are
stored in
niden so for now most of the executions
that we've been working with have been
manual executions in these past videos
we've been clicking on execute workflow
or test step but as mentioned previously
when we activate a workflow we're going
to be turning on the Automation and so
the workflow is going to execute behind
the scenes without us necessarily seeing
the execution of the workflow
we can access the
um history of all of our workflows
executions in what we call the execution
log which you can access by clicking on
all
executions by default successful and
failed production executions so when
workflows are activated those are saved
and manual executions are not
saved however on a per workflow basis
you can activate the logging of manual
executions you can sort the execution
log by workflow status date ranges or
even execution data uh we'll cover how
to store execution data in the logs in
the advanced
course for the execution log you can
access the
individual execution histories um so
here you can see an example of an
execution history we can see the
schedule trigger that activated we read
the data in the sheets and depending on
if the email existed or not we messaged
sales or
marketing these are static because they
are past executions and so they cannot
be changed this is a snapshot of the
final state of all of the nodes from
that given
execution this is very useful when
debugging fixing or inspecting workflows
you can look through past executions to
understand how the
workflow
works you can open of course each node
by double clicking on it to see what the
input and output data was as well as as
all of the node settings and if the node
has an error you will also get the
details about that error um from the um
from double clicking on the
Node so as we mentioned earlier
sometimes executions can fail you're
going to activate a workflow or push it
to production and sometimes your
settings aren't going to be optimal or
your input data is going to be in the
wrong format and this is going to cause
your workflow to fail so there can be
multiple reasons that workflows fail and
it's important to understand what kinds
of Errors there are and especially how
to fix
them the first method of handling errors
and this is a very important one is to
use the error workflow the error
workflow is a workflow that is executed
as soon as a node has an error and this
workflow allows you to report on any
workflows that have errors pretty much
informing you when your workflow needs
to
be uh debugged or fixed because it
failed this workflow needs to be
configured for every new
workflow so when building workflows make
sure to set your error workflow um you
can have one or multiple error workflows
in your NN
instance here we have an example uh this
workflow is launched every day at 8:
a.m. it reads contacts in a Google
Sheets and updates the CRM in this case
Salesforce with those
contacts the email field is mandatory in
Salesforce and so it will cause an error
when trying to create a contact for
items that do not have an email the this
triggers the error workflow with
information on the Node that had an
error in this case the Salesforce
node so here of the 10 items that we
read in the sheet some of them didn't
have an email and this Co caused the
Salesforce node to um have an
error another way to trigger the error
workflow is with the Stop and error node
and this uh node is um will raise an
error message every time that it is uh
triggered every time that it is executed
and you can configure that specific
error
message once triggered you have multiple
options by default the whole workflow
will be stopped and its status will be
set to failed um however we'll look at
this um in N you can choose the behavior
of the Stop and error
node you can use this to manage edge
cases in your workflows by raising
errors when certain conditions are met
that should not be
met this workflow raises an error when
the email data from the web Hook is not
valid so here we check if the web hook
contains a valid email and if
not valid we stop and
error uh the error workflow is created
using the error trigger node that we'll
take a look at in a second this node
will output any uh and all information
related to the workflow so it will
contain the name and workflow ID the
execution ID as well as the links to
find that specific execution that has an
error um as well as information about
the node that had an error so either a
specific node or the Stop and error
node we always recommend having uh a
place usually a channel this can be
slack WhatsApp teams or any
communication tool where you can report
on errors so people in your team with in
and access can see that the workflow had
an error and quickly jump in to debug
and fix the workflow so here you have an
example error workflow an error trigger
that sends a slack message an email or a
telegram you probably don't need all
three but you probably need somewhere
where you can report on errors in uh
your
workflows let's jump into NN um and see
how we can use errors and build a super
simple error workflow
so here we are back in NN and let's
build a super simple error workflow so
we're going to call it error workflow
and the first step we're going to add is
an error
trigger when we add the error trigger we
can fetch a test event to understand you
know what kind of data uh we will
receive when the error trigger is well
triggered so in this example we're just
going to build a very simple error
workflow with u a slack message a
containing information about the error
so that someone can go and quickly take
care of it and
debug so here we have our error trigger
and our test events I can just come in
here and as usual add a slack node to
send a message to myself user
Maxim and here just to test we're going
to say
and it an error then we're going to
include some
information so
workflow comma sorry colon the name
we're also going to
include the UR the
execution URL
here and then going to include the error
message error
message this way every time that we have
an error and the error workflow is
executed we instantly know which
workflow has an
error then we have a very quick link to
access the specific execution so we can
go into NN and see what the error was
and we also have an error message uh so
we can better understand what that issue
was so here I can test the step receive
the message and once we've tested I can
change and here not send to a user but
send instead to a channel and here I
have
a and then
errors and any an errors Channel again I
can test that step and every time from
now on one of my workflows has an error
and the error workf is cold I will get
that message in uh anent errors Channel
with the information about that
execution in the advanced course we'll
cover a bit more of a complex error
workflow uh specifically with um tagging
people tagging the owners um so we can
get the right people to take a look at
uh the errored
execution now jumping into uh the
workflow from the previous video uh just
cleaned up a little bit instead of
having those two ifs I put a switch in
place and I renamed the nodes so it's a
little bit clearer again we have our web
hook input data with the event type and
information about the contact and
depending on if the event is equal to
account created or team member invited
we're going to be sending different
messages here we might want to handle
some potential errors so there are two
main potential errors that I could
imagine first we're looking at the email
and second of all we're checking what
kind of event uh we're handling so there
are two kinds of error handling methods
we could use here the first one is to
check if the email is valid so here we
could do a pretty basic check and say is
does the
email contain an at symbol that could be
the maybe simplest way of making sure we
have a valid email more complex ways
could be to use uh reg xes make to make
sure that um the email matches a very
specific email format for now let's just
um check if the email
contains an at symbol so for example
this would be email valid if the email
is valid we can just continue the
workflow and if the email is not valid
then we can add a stop and error and
here we can throw an error message or an
error object simply
mentioning um invalid email
then here we have settings on sorry we
have a switch on the event type we could
also imagine that we might not have a
valid event so here in the switch I
could add a new crowding
Rule and this could be
if the event
is does not exist or as we saw earlier
if the
events as an expression
obviously is equal to an empty string
and both of these
outputs we could then drag to a stop and
error and this time instead of
having invalid email we could say
invalid event
now we know that when this workflow
activates it'll handle both the case of
an invalid email and the case of an
invalid event so if we do have any
errors in our web hook uh system that
will trigger error workflows and then we
can go in and check why did we not have
an email or why did we have an invalid
event one more thing to mention here in
the Stop and error workflows if we head
over to the settings where going to be
able to select what kind of behavior we
want on error as mentioned in the slides
by default this will stop the workflow
but if you are handling with an edge
case that doesn't prevent the workflow
from continuing you might want to
continue or continue using an error
output um so this can be configured on a
node per node basis both for stop and
error workflows as well as uh just usual
um noes
thanks for listening to the seventh
video of the idend beginner course where
we looked into execution histories error
handling and a very basic error workflow
in the next video we'll be covering how
to debug a workflow to avoid it from
having errors again as well as retrying
see you in the next video
