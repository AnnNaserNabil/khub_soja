hello and welcome to video number four
of the beginner course of nen um in this
video we'll be covering what kinds of
data nodes use and how the data is
passed from node to
node this video is going to be a bit of
a longer one um and we'll cover some
more technical Concepts but it is very
important that you understand these
Concepts um so that you can build the
best workflows possible
possible let's start by explaining some
core data Concepts that you need to
understand to make the most out of
nen there are two main data structures
that we are going to take a look at
today the first one is what we call a
Json and the second one is a list Json
are a very common way of storing data
digitally and are written between braces
or curly
brackets they are made up of key value
pairs um each one being separated by a
comma lists are nothing more than a
collection of objects um they can be of
the same or different type written
between brackets or Square braces um
also separated by
commas a Json can be embedded what we
mean by this is sometimes we will have a
Json where the value of a key is equal
to another Json we can use this to
organize complex
data in this example we can use an
embedded Json to group all the
information about Emily's location into
a location key this key contains itself
AJ
sorry the value of this key is a Json
that has two keys with information on
M's country and
city to access the data in a Json we can
use the standard dot notation here doter
Json let lets us access the Json itself
and by T
typing first uncore name we can access
the value of the first uncore name
key for embedded Json we can use
multiple dot notations in a row here to
get the
location we can write dollar
json.
location.
country Dollar json. location being
itself ajon on which we use the dot
notation to to get the value of its key
country lists are uh simp are simply a
collection of objects uh we can have a
mix of letters and numbers uh we could
have a list of any type of objects and
as you can see here written between
square brackets and separated by
commas and because Json themselves are
OB objects we can obviously make a list
of
Json here we have the example Json from
earlier separated by a comma and then
two more Json that form well a list of
Json as we can see here we have the
square brackets at the top our first
Json a comma our second Json comma our
third Json and then finally our last uh
brace
there is a very interesting
correspondence between Json and tables
where we can tell that one Json is
equivalent to one row with the keys
being the headers of those
rows this might remind you a little bit
the Json and table view that we saw in
the previous video on ENT
nodes here um when we have a list of
Json we can see that Emily here first
name Emily last name Johnson email and
her email fits very well into the row
format where we have the different keys
that are the different Columns of our
table so we can imagine that if we have
a list of three Json then we have an
equivalent table with three different
rows each one with the values of the
corresponding
Row in nen this is what we call items
here in red we have an example of an
item we have the first item of the list
and node nodes use items plural as
inputs and as outputs these are the only
accepted formats for node inputs and
node
outputs even should we decide to return
nothing no information we still have to
return a list with an empty Json that
would be considered the empty output for
an nadn
node so now let's look into how nodes
actually use these items when we're
going to be executing them and building
workflows each node executes one per
item in the input data so there are some
exceptions uh that we will be covering
in the advanced course but in general
you can remember that each node executes
once per item for example here we are
using the date and time node to format
different dates the node will read each
item format the date and then return it
as an individual
item this is how we build workflows by
transitioning items through different
nodes so here if we look into this
screenshot here we have a list of
items each with different
dates and here as the output we have
again three items because it was
executed once per item and then we have
the formatted date which is the same
Associated date but in a different
format in the advanced sorry in the
parameters of um the node we can access
the additional settings through the gear
icon and here we can decide to execute
only once where a node will execute only
for the first item of its
input to better understand how uh
workflows actually work um here we have
an execution schema of a very simple
workflow the workflow starts when
clicking execute workflow reads data
from a Google sheet and then filters the
items depending on specific
conditions the execute workflow node
launches and returns an empty Json so no
data at all all and here we can see we
have our empty Json in a
list this is returned so that the next
node can execute once because we have
one
item the Google Sheets node then
executes once and reads three items in
the Google sheet that it's reading from
and so it returns three items Json one
Json 2 and Json 3 each um being linked
to the corresponding Row in the Google
sheet so we can see it outputs three
items the filter node checks if each
item satisfies a specific filter
condition here we can see that only one
of the items does and so it only outputs
one item
when building workflows the whole point
is to configure a node to execute
depending on what's in the input data we
can do this by dragging the key from the
table Json or schema
view this will create what we call an
expression Expressions will for each
item return the associated value
to that
key the NN interface will show you an
example value from the first
item so using the
same uh data as before in the schema
view we can drag the first name key into
the filter
conditions and I can say I only want to
allow through the filter if the first
name is equal to something for example
Emily here we have the expression
editor and we have an example result for
item one so here json. first name of
item one is equal to
Emily remember nodes execute once per
item and so as it executes es for each
item the expression would have the value
Associated to that
key everything between two braces or
curly brackets is an expression so we
can use expressions for many different
use cases we can use item variables as
we saw before so dollar json. first name
json. last name or email we can also use
JavaScript so if we want to apply a
JavaScript function or a JavaScript
method to for example one of the items
values then we can use any of the
built-in JavaScript functions or methods
between the curly braces
we can also in Expressions combine one
or multiple Expressions as well as plain
text so here we have uh using the slack
node we have an example where we want to
send for each item the first name space
last name and then between parenthesis
the email followed by a short message
that will be the same for every person
just signed up to Acme here we have an
example of what the message would look
like for the first
item we can see that if we execute this
node and we have three input items we
will get uh one message that is going to
be um different for each item in the
input jumping back into NN we can cover
um how to use these Expressions uh and
look at it together in the tool so here
we have the workflow that we built in
our last video a manual execution
followed by reading some data in a
Google sheet and I'm just going to click
test workflow so we have the data
already
executed here again using the plus up
here or the little Plus at the end of
the node we can add a new node this time
we're going to use the edit Fields node
that we'll cover more in detail in the
next video but just to show you a little
bit how we can use
Expressions we'll leave the node
configure as it is by default um and
just add a new field that we're going to
call full name so obviously full name is
going to be first name followed by last
name so I can just drag the first name
drag the last name and if
um I click test step we're going to see
that each item is going to have now a
full name field with their first name
followed by their last name here in the
Json view we'll see it a bit better Paul
Harris Paul Harris Marcus Bennett Marcus
Bennett in uh the Expressions here um as
mentioned in the slides we can see an
example of what the result would be for
the first item so Paul Harris what we
see here and in uh the expression we can
also add a little bit of JavaScript for
example I could want the last name to be
two uppercase um so this would make the
first name as is in the input data and
the last name would be turned into
uppercase so here if I execute this um
if I test this step we can see that now
we not only have the full name but we
have the full name with the last name
that is
uppercase thanks for listening to the
fourth video of the Inn beginner course
where we covered the key data concepts
of nadn in our next video we'll be using
everything we learned in these videos to
finally start building our first
workflows if you you are working on a
workflow and have a hard time
understanding why your input and output
data are the way they are feel free to
come back to this video and make sure
you have a thorough understanding of
both items and
um lists and Json as these are key
Concepts to understand um when building
workflows and using
Expressions see you in the next video