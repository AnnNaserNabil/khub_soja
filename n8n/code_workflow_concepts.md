hello and welcome to video number five
of the beginner course for nen in this
video we'll be covering some core
workflow Concepts and continue building
our first workflow
together let's start off by covering
some uh important core workflow Concepts
that you'll need to understand before
building uh your own
workflows here we have uh the canvas so
from the canvas we can see the main
workflow elements up here we have the
workflow menu uh the name and tags
associated with that workflow we have
the workflow activation settings here we
can also access the version history and
workflow specific uh settings up in the
top right corner in the middle obviously
we have the nodes of the given workflow
and we have some Zoom settings uh to the
bottom left
activating the workflow is what allows
us to push a workflow to production and
is what actually allows us to start
using the workflow automatically uh
we'll cover workflow activation a little
bit later in any then and of course in
the Cannabis we have the nodes which are
going to be the biding blocks the atoms
of our
workflow the main workflow menu is where
you'll find all the workflows of your
Ann instance you'll find information
like tags and owners that you can filter
on um you can also find all workflows of
which you are the Creator uh by clicking
my workflows down here to see only
workflows owned by you you can create a
workflow uh and every time you do you
are going to be assigned as the default
owner from the workflow setting so to
the top right of the canvas we can
access everything that has to do with
workflow accessibility as well as the
error workflow and how workflow
executions are saved um we'll be
covering both error workflows and saved
executions in video number
seven let's look at how nodes are
connected um and how we actually can
create
workflows every workflow starts with a
trigger node you can recognize them by
the fact that they only have an output
Branch as you can see
here and they have the orange lightning
icon next to them a workflow can have
multiple different triggers multiple of
the same Trigger or multiple different
triggers for more complex use cases and
the trigger is what starts the workflow
except when testing the workflow it must
be activated for the trigger to actually
work in the canvas when we double click
on a node we can see the nodes before
and after the given node um you can use
these little icons to navigate through
the workflow as you are building here we
have a very simple example uh when
clicking execute workflow Google Sheets
filter and then an edit Fields node and
by double clicking the filter node we
can see to the left Google Sheets and to
the right the edit filter
node another core topic we need to cover
is branching so branching is very
important uh when building complex
workflows um branches are how we create
different paths or different set of sets
of actions depending on different
conditions um branches is what allows us
to create complex
workflows where one workflow is able to
cover a large variety of cases and not
only one single
case we can create branches in two
different ways the first way is we use a
node that has multiple output options in
this case each item will follow only one
of the multiple paths so here for
example we have we have an if node and
the if node has two outputs true and
false as we can see we have three input
items and only one item goes through
true and two items go through false so
each item only follows one specific
path another way to create branches is
by dragging two or more output lines
from a single node
this means every single item will follow
every path and will be duplicated out by
the number of paths we'll um see what
this looks like in N then in just a
minute nodes with multiple output
branches will have different sets of
output items obviously um and these can
be accessed from the
output data of the given node for
example here the if node has an output
item for items that satisfy the
condition and an output item for items
that don't satisfy the
condition let's jump into naden and see
what all of this looks
like so here we are back in nen uh the
first thing I'd like to show you is uh
triggers and Activation so this workflow
that we started building that we execute
manually before reading some information
in a Google sheet if we want this
workflow to execute let's say every
morning at 8 a we could use the schedule
trigger and this would allow us
to set the uh trigger interval how many
days between each trigger and what hour
to trigger at so in this case we would
like to trigger every day with you know
every one one day as spacing and we're
going to trigger it at 8:00
a.m. here you can see we have so
multiple Triggers on a given workflow
and here if I want to test the step it
is going to execute even though it is
not 8:00 a.m. on a given
day however if I want
to uh once I'm done building this
workflow if I wanted to actually run
every day at 8 am I have to make sure
that the workflow is activated so when
the workflow is activated you will get a
confirmation message your schedule
trigger will now trigger executions on
the schedule you have
defined and now I could
remove this uh manual
step every time you activate a workflow
it will automatically save um so make
sure that your uh workflow is ready to
be activated before you actually do turn
it
on so continuing to build this
workflow let's add a node after the uh
Google Sheets first let's execute so we
have uh the data just as a reminder we
have here a list of contacts with their
first name last name email and Company
so what we'd like to do here is first of
all we'd like to remove everyone that
doesn't have an email let's say we want
to email all of these people if people
do not have an email then there's
nothing we can do with them so we are
going to use the filter node to filter
out every contact that does not have an
email so here in the filter node we have
our conditions and here we want to make
sure that the
email is so email is going to be a
string a sequence of letters and
characters and we want to make sure that
it
exists so by executing this step we can
see that we went from 10 to 10 items in
this
case we can see that even if the email
is empty we still have an empty value so
in this case the condition would not be
exists but
rather is not equal to an empty string
here if we test this step again we will
see that we go down from 10 items in the
input to eight items that were kept and
two items that were
discarded items that had empty emails so
from here on we only have the eight
items uh that we want to work with that
have
emails from here we could think okay
maybe we want a different kind of
behavior depending depending on if the
uh person has a professional email
address or a work email address or a
personal email address so here we can
add an if
node and in the if node we can add some
conditions so looking at the Json we see
here a Gmail address some work
emails and a hot mail
address so our condition here is going
to be we want the email
to not
contain
gmail.com this will allow us to filter
out the Gmail
addresses and as a second step we could
filter
out so here we see we have gmail. and we
have a hotmail.com
so by simplifying this condition by
removing the do com we could just remove
anything that has at Gmail and we can
add a condition when we add a condition
uh and so we have two conditions in our
if node we have to decide how we want to
combine the
conditions here we want to exclude
everyone whose email contains at
gmail or anyone whose email contain PS
at Hotmail and so in this case we're
going to be using the or filter so again
we drag and drop email and we want it to
not
contain hot mail or actually at Hotmail
would be
better if we test this
step so if we test this step we can see
that none of the items actually went
into the force branch and this is
because
we are creating the uh branch of
professional email addresses and so we
actually should use the and condition
the professional email addresses are
that they don't contain Gmail and that
they don't contain hot mail so here I
can test this
step here we can see that we have uh
multiple branches so if I were to
add a node here and
then another one
here and I execute again this if node
we're going to see five items are going
up this branch and three items are going
down this
Branch if we had uh this allows us to
create these branches where each item of
the input is going down only one of
these
paths if we were to uh just drag another
if
from the filter
node and then maybe just realign
things so we can see a bit
better and here I execute the filter
node we can see that in this case eight
items are going down here and eight
items are going down here so by dragging
multiple outputs from the same node we
duplicate the items down both paths but
by creating an if or a set of
conditional branches then we split the
items down the different
paths thanks for listening to the fifth
video of the nend beginner course where
we covered some core workflow Concepts
and created our first connected nodes in
the next video we'll be covering some
very useful nodes when building
workflows and we'll keep on building on
this workflow to make a more complex
example see you in the next video