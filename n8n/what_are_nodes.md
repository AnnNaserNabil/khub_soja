hello and welcome to video number three
of the beginner course of nen in this
video we'll be covering the node in
naden which is the building block of all
of your
workflows let's start off by looking
into what an niden node is and the
different types and categories of niden
nodes the node is really the atom of nen
and every workflow is built by
connecting consecutive nodes there are
three main categories of nodes entry
points which are going to be your
triggers functions that allow you to
transform filter or format data and exit
points that are going to be your apps or
applications as we mentioned
before in nen you will find them grouped
by type when adding them to the canvas
triggers actions in app data transer
information like filter flow files and
advanced nodes here we have an example
of the Google Sheets
node when you open nen you start on an
empty canvas this is where we will be
adding our nodes to build our
workflows we can click on the add first
step or the plus button at the top right
of our screen to add the first node
in this case because it is the first
node of our workflow we are prompted to
add a trigger node there are the same
triggers that we mentioned in the First
videos by clicking the onapp event
option we will see a list of
applications that can be used to trigger
the
workflow for now we can start by
launching the workflow manually and
therefore selecting the manually
trigger for all following nodes we will
have the list of different types of
nodes as you can see here instead of
scrolling through the list you can also
start typing the name of the node or
application that you want to use to
access it
quickly When selecting the node you will
sometimes be prompted to pick a specific
action from the list of available
options
this is also called the operation here
we can see the Google Sheets node has
different actions that are possible such
as append or update row clear sheet
create sheet delete sheet
Etc once the node is on the canvas you
will be able to access its settings we
can click the play button to execute the
node as well as common options like
duplicating renaming deleting
Etc double clicking the node will open
its settings this is where we can set up
the node for execution there are two
types of configurations the parameters
which are the default view when double
clicking on a node these are going to be
specific to a given node and operation
for example selecting which spreadsheet
and Sheet we want to read data from when
we select the get rows operation in
Google Sheets as you can see
here the gear on the top right gives us
access to advanced settings that are
node independent like notes or visuals
and execution settings we will cover
these um in the advanced
course at the top of the parameters for
each app node there will be an option to
set your
credentials this is a very important
concept to understand this setting is
how we authenticate to different
applications and services as mentioned
in the apis and web hooks
video they are saved at an instance
level to make sure workflow building is
efficient and can be shared to specific
users or workflows to um ensure security
around the accessibility of apps you can
prevent specific people from having
access to specific
credentials to the left and right of the
notes parameters we have input and
output
data this makes it easy to understand
what data is being read as the input by
the node and what is the associated
output data
using an example Google sheet we can see
three main
views this is going to be the table view
with the different columns and values
Associated Json view where we can see
the output data as key value pairs this
is going to be the topic of our next
video and schema view where we can see
all the different keys from the input
and example of corresponding
values let's jump into nen and see what
all of this looks
like here we are in niden and we can see
an empty canvas for our
workflow just as we saw in the slides
let's run through adding a Google sheet
reading its data and looking through the
different views um in
nen so here we have a Google sheet that
contains demo data we have a column with
first names a column with last names
emails and company names we can also see
here that we have two sheets one sheet
named data and one sheet with company
info in naden we can click add first
step and as mentioned in the video
choose to um trigger this work workflow
manually from here we can either using
the plus button up here select the next
node or clicking the plus button here we
can add a node that is connected
automatically to this
trigger here I can choose action in app
and look for Google Sheets but it will
be faster if we just type Google Sheets
when I click on Google Sheets I have
access to all of the different actions
and triggers associated in this case
we're going to get a row in the sheet
get one or multiple rows in the sheet
because we want to read data from the
Google
sheet here we can see that I already
have a credential that is set up
um these are um as mentioned earlier
saved at an instance level but let me
just create a new one for the example if
I create a new credential I can choose
to use o or a service account in this
case oo will allow me to very easily and
quickly connect um and read data from
this sheet so here I'm going to sign in
with Google
and now I'm signed in this means that
Nidan has access to all of the data in
my um any in my Google Sheets account
and now I
can um choose the resource I want to act
upon so this is going to be a sheet
within a document and the operation is
going to be get
rows if you decide you want to change
your operation you can also select from
a list of given operations
here and now I'm going to select which
document and sheet I want to read so
here I have multiple options I can
either enter the URL or the ID um the
easiest here is going to be obviously to
select from the list so here I can
choose any then demo data and then from
again the list of sheets I have the data
sheet and the company info sheet so I'm
going to select the the data and execute
the
node here we can see to the right we
have all of the information from the
Google sheet as well as a column that
contains data on the row number um all
of the information that was in the
sheet is now available here in nen
fields that did not have values are
going to be empty this is as we saw
earlier the table view which is going to
be um very useful when reading Google
Sheets we also have the Json view with
each line corresponding to one Json here
again we'll cover Json in our next video
and the scheme view as mentioned
earlier from here we can also access the
node settings um as mentioned a little
bit earlier these are going to be node
independent so whichever node uh you are
accessing these settings of these would
always be the same we have um settings
that depend on what the type
of from here we can also access the node
settings as mentioned earlier the node
settings are node independent so
whichever node you are currently editing
you will have the same node settings we
have settings that uh pertain to the uh
execution and output of the node so the
node can always output data or execute
once um and not the number of times
of from here we can also access the node
settings uh as mentioned previously uh
the settings are node independent and so
no matter which workflow you are editing
you will have access to the same
settings um so in these settings have to
do with the node execution or output um
so here we can always output data or
decide to only execute once um also uh
retry on fail this can be very useful
when um using apps that behind the
scenes uh actually use apis and then we
have error settings what do we do in
case of an error notes and if we want to
display notes in the flow I'm adding a
note to explain what the workflow does
um helps a lot when trying to understand
uh
workflows here under the uh sheet
settings we have additional options so I
could decide for example to filter if I
want the email uh to be a specific value
or the first name to be a specific
value and we have additional options
such as data location output formatting
or how to deal with filter that has
multiple
matches uh many nodes are going to have
additional settings that are available
here at the
bottom additional filters or options for
example here we can also see the input
data which in this case is going to be
empty because we using the when clicking
test
workflow but when we're going to be
building our workflows to the left we're
going to have our input data in again
the table Json or schema View and to the
right we're going to have our output
data thank you for listening to the
third video of the nen beginner course
where we covered nodes and how to use
them in the next video we'll we'll dive
deeper into what kind of data nend nodes
use um allowing us to understand the
input and output of the nodes and how
this data flows between different nodes
with our final goal being to create
workflows see you in the next video