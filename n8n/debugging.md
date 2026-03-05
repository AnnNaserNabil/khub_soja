welcome to video number eight and our
final video of the beginner course for
nadn in this video we'll be covering how
to debug workflows so in the previous
video we talked about error handling how
sometimes workflows when they are push
pushed to production when they are
activated can encounter errors and
debugging is the process of fixing those
errors and making sure that they don't
uh happen
again so let's start by talking a little
bit about what debugging is and why it's
an important skill uh to
master so when workflows or rather when
specific nodes within a workflow fail it
can be for many different reasons they
can be configured wrong the uh
underlying service can be unavailable
for example um if you are using Google
Sheets or slack you might get a 500
error meaning that the service is simply
unavailable at the moment or problems
can be related to input data so let's
say you're receiving a web hook the web
hook can be missing information causing
the workflow or one of the specific
nodes to
fail by default this stops the workflow
from finishing its execution and it sets
its status to f
failed from the execution history or
execution log we can find the list of
all of the workflow executions that
failed and then debug them one by one to
make sure the errors don't happen
again it is important to note that
sometimes a workflow can fail from an
automation standpoint without
necessarily being tagged as failed in
the sense that if you are trying to
automate a certain task but no node had
an error then sometimes the task will
not be automated and the workflow won't
be tagged as an error so this is why
error handling is extremely important uh
to make sure that your workflows execute
correctly and uh we'll be covering an
example of this in just a
minute the easiest way to debug bug your
workflows is by using the debug in
editor feature in nen um this is a super
powerful feature that lets you pin data
from an execution history into the
current canvas of the
workflow what this does it is is it
effectively copies over whatever data
was in or whatever items were in the
failed
execution and pins them into your
current workflows editor so that then
you can use this data to
debug the same way that when we um used
the pin feature in the web hook node uh
a few videos ago this is the same thing
it allows you to PIN error data to uh
your workflow
canvas pin data will have a blue or
purple symbol in the bottom right corner
and workflows can only have one set of
pinned data at a time so you have to
work through uh different types of bugs
one by one uh making sure that um you
are fixing them all and that by
repairing some you aren't creating other
bugs once the error is fixed or handled
um we can use the retry feature to
trigger again all of the failed
executions this is a super useful
feature because
when um a workflow fails we might have
five or 10 executions that were
unsuccessful and from the execution log
you can decide whether you want to retry
with currently saved workflow or with
the original workflow from the time of
execution the retry is executed from the
node with an error uh this means that if
the error in your workflow comes from a
wrongly configured node previous to the
one with an error you will have to use
the copy to editor node to re-execute
these because it only executes from the
errored
node another useful feature when
debugging is the edit out output
feature the edit output feature lets you
manually edit the output for a specific
node while it come while it can come in
handy when testing or debugging
workflows um specifically if you're
using web Hooks and you don't want to
have to send uh many different kinds of
tests events um without requiring you to
execute all of the past nodes and uh do
all your Transformations on the data uh
it should be used sparingly as it is not
a very scalable method however in cases
where retrying is not possible it can be
a quick way to fix a backlog of a few
executions another very very useful
feature when debugging is the workflow
version history so when making updates
to workflows error handling or debugging
sometimes it happens we make mistakes
and we can lose sight of where we
started from luckily we have the version
workflow version
history from here you can see all of the
previous versions of a given workflow
that were saved uh this can be useful if
you need to revert some changes that
were made to a workflow that might be
causing bugs or to inspect the structure
of a previous workflow version of the
workflow uh this can be combined nicely
with the retry feature if you need to
revert to a previous version and then
retry multiple executions with the
currently saved
version let's jump into nen and do two
different examples of debugging a
workflow so here we are in nen and uh we
have this workflow that we're going to
uh debug the first step of debugging any
workflow is well first of all
understanding what the workflow is
supposed to do when it executes properly
so here we can see uh we have our
execution logs we have a successful
execution a failed execution but let's
first look at the successful execution
to sort of figure out what the workflow
does so here we can see we receive a web
hook in the web hook we have an ID key
with a specific
identifier then we get the user so this
is another function of the Google Sheets
we're going to be uh getting rows but
specifically filtering on the ID column
and we're going to be uh looking pretty
much for the row with the specific ID uh
this returns all of the row
information uh all of the information
from the different columns in the sheet
so we get an email first name last name
and a company as well as the ID that was
the same one as we filtered on and then
we send a slack message with info for
the specific email their first name last
name and
Company now we can look into the failed
execution here we can see uh quickly by
looking uh we have an error error cannot
read properties of undefined reading to
string so clearly here it tried to read
the body. ID and in uh the web hook we
can see that this web hook did not have
an
ID so what we're going to do is we're
going to click debuging Editor to copy
the data over and we're going to deal
with the different cases so first of all
we could say we could add an if and make
sure that we have a valid uh ID so
here we want the json. body. ID this
time to be to exist and if this is the
case then we can go up this path and get
the user and send the slack
message here there are multiple ways we
could continue debuging message
either we could say if we don't have an
ID so here let me rename ID if we don't
have an ID we could for example send a
slack message
saying
um web hook received did not have ID uh
and that way we can look into
it but we could also be a bit smarter
here and see the goal of this workflow
is to look up a user in uh the specific
database and here it's with the ID but
we also saw by looking at the uh past
execution that in the database we have
an email so instead of looking up the ID
if we don't have an ID but we have an
email we could try looking up using the
email so going back to the editor we
have the case with a valid
ID then we could say we have the case
where we have an
email and this would be json.
body. email
exists and then all we would have to do
is do the same
actions instead
of looking up the ID we would be looking
up the email
json. body. email and then we would send
the same slack message with the
information from uh the
contact what we can do from here is then
deal with the case where we have none of
them this would be a great place to use
the Stop and throw error um node
here and a little message no ID or email
from here we have a fixed workflow so
first of all we can verify that with
this error data it still works so I can
test the workflow and as we can see here
it didn't have an ID so it went down to
the email field it had a valid email so
it got the user and it sent the
message in this specific case we
wouldn't be able to retry this work flow
because we made we had to make the
modifications before this um specific uh
node another case I'd like to cover is
this
execution what we can see here is we
received the web hook we tried to read
the user
and nothing happened here no message was
executed and what this is what I was
mentioning earlier where sometimes a
workflow can not fail but can also not
be successful and this is a very good
example of this so what happened here
well clearly we tried to look up someone
with an
ID that we can see here in the input
data but no data was returned so
something that we can do here
is
again this time it won't be debug in
editor but copy to editor because
um it wasn't
errored and what we can do this time is
now deal with the case with this case so
if we test the
workflow we can see that the updates we
made still work with this execution
except we're still not getting the
output we wanted so here something we
can do is in this get user um sheet by
heading over to the parameters we can
um set Sorry by heading over to the
settings we can ask it to always output
data and this means that even if it
doesn't find a contact here we'll
have
um here we'll have the information of an
empty
item from here before sending the slack
message we can set an additional
check so here we could say does the
contact have an
email so this would
be by reading the information we would
have here in
slack json.
email if we have an
email exists then we send the slack
message here we would have to do the
same modifications so copy this
put it here we also
check and if we have a valid email we
send the message in the cases where we
don't have a valid email and so there
was the lookup was unsuccessful we could
just uh drag a stop and throw
error
contact not found in
database and this we can do it this way
here we could sort of factor this a
little bit better seeing as all of these
do the same thing we could just use a
single if and a single throw
error by attaching this one
here this way we have debugged the
workflow and
whether if we can't find the user next
time we'll have an error someone can hop
in
and deal with the case where we don't
have a user if this is usual then we
handle the case differently if it's not
usual we can figure out what's
wrong thanks for listening to the eighth
and final video of the end beginner
course where we looked into debugging
workflows and fixing failed executions
in the advanced course we'll be covering
some much more advanced topics Advanced
workflow building complex data flow
those more advanced examples as well as
well as error handling and
debugging thanks for your attention and
for some of you see you in the advanced
course