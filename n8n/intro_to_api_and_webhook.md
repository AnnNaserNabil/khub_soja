hello and welcome to the second video of
the NN beginner course today we will be
talking about apis and web hooks two
very important Notions that you need to
understand before building your first
ended end workflows in this video we
will be
covering what an API is as well as a
definition and an explanation of its
main components and we will be talking
about web hooks sometimes called reverse
apis so first of all what is an API I'd
like to start by explaining an analogy
that is going to be very useful for
understanding not only what an API is
but also the different parts and names
Associated to them so imagine you
sitting at a restaurant and you go and
you sit at a table how do you get food
well you ask a waiter and the waiter is
going to to take your order and bring it
to the kitchen once the kitchen is done
preparing your food the waiter will
bring back your food to the
table this is very very similar to how
an API Works um we're going to be using
uh this analogy to explain the different
parts of an API um so keep this in mind
while we go through this
video a technical definition of an API
so an API is an application programming
interface it exposes a service and
developers write programs to consume it
so if we go back and we think of the
actions or the apps in the past video we
might take the example of Google Sheets
so Google Sheets has an API and in the
API we have different services so one of
these service might be get all of the
data in a specific sheet and so the
Google Sheets API exposes the service
that allows you to read data in the
sheet developers write programmers
programs to consume it so using edn
we're going to be able to consume the
Google Sheets API using the different
services for example updating rows or
getting data in a
sheet using this metaphor uh we can see
that the waiter is what's going to be
called the interface and the application
is going to be the kitchen so in this
case the application might
be the Google Sheets and then we're
going to be using the interface to
interact with the
application why do we need this
interface to interact with the
application well it allows us to
abstract the complexity
imagine if every time you went to a
restaurant instead of just sitting down
and ordering food you had to go to the
kitchen and explain your order wait for
it to be done and then take your food
back to your table this would be much
more complicated than just asking the
waiter for your food the same thing goes
for applications if every time you
wanted to read data in a Google sheet
you had to go to Google servers find
your specific
sheet and then read the data from it
that would be much more complicated than
just using the API which gives you a
very abstract but easy way to access the
data from the
application so how does an API work it
uses what's called documentation so
documentation explains how the
application programming interface or API
works
and using the restaurant um analogy it
in this case would be the
menu so a little bit of
terminology we send a request through
the interface to the application and the
application uses the interface to send a
response we also have the client server
Notions in this case you would be the
client and the application and the
interface would be the server so keep
these in mind when you see these
terms when using uh apis we have as
mentioned earlier requests and response
so we're going to break down the
different components of a request and
the different components of a response
starting with a request there are four
main components to an HTTP request in
these videos we're only going to be
talking about HTTP requests there are
different Frameworks to make API
requests such as graphql but most of the
apis that you're going to use are going
to use the HTTP
framework everything that we're going to
talk about today is mirrored in the HTTP
request node the HTTP request node
allows you to make HTTP requests in nen
and receive the responses this is going
to be very useful when building
automations that need to use specific
apis
there are four components to an HTTP
request there is the URL the method the
header and the body let's look at each
one of them the URL is the unique
location for a resource on the web this
can be a page and image a PDF or some
data here we have an example of a URL
you can see a scheme a host a port a
path and some query
parameters the scheme The Host and the
path are going to be mandatory and the
port and query parameters are going to
be
optional something that is important to
note query parameters are always
preceded by a question
mark then we have the method the method
describes the action that we want to
perform at the given URL there are two
main methods that we're going to be
using for HTTP requests there is the get
and the post method methods get most of
the time allows us to receive
information so if you're reading data in
a Google sheet you're using the get
method and post is going to allow us to
send information so if we want to send
information from form submission we're
going to be using the post HTTP
method the other methods are a little
bit more rare uh we have for example
delete put and patch that are less
common but can be
used what's very interesting with the
method is there are all the time verbs
which means that they describe very
clearly what we're trying to do so when
wondering which method you need to use
think of which verb would be most
appropriate to the action you're trying
to
accomplish then we have the header the
header gives us a little bit more detail
or context for a given request
so for
example common Lo common information
that you will find in a header is going
to be your location or your language
preference or your device type um every
time you open a page on the internet
you're making an API request to a server
and the server is responding with the
web page so if you are browsing the
internet on your computer or on your
laptop top you're going to have
different information or context in the
header of your
request an example of a header would be
accept application jjon which tells the
server that it would like the response
of the HTTP request in the Json
format then we have the body the body is
optional and only exists for post
requests it contains the information we
would like to send to the server or
application so if we take the example of
form
submission then the body might
contain first name Maxim last name poon
and then the associated email so this is
the information that we are sending to
the
server finally we have credentials so
the credential wasn't listed as a main
part of the HTTP request because it
isn't um a part on its own there are
many different ways to use credentials a
credential is how we let the application
know that we are allowed to make a given
request as you can imagine if anyone
could read your Google Sheets or update
your Google Sheets or send messages on
slack that would be very dangerous so we
include credentials in our HTTP requests
to indicate to the server I am
authorized to make this
request most apis require authentication
through credentials however there are
some apis that do not require
authentication the two most common ways
to authenticate to a service are going
to be through query parameters question
mark API key equals followed by the API
key or header authentication for example
authorization
colon Bearer followed by the API key
another very common way to authenticate
to a service is through oo every time
you click sign in with Google and the
little window opens and you sign in with
your Google account this is an o or
authentication
method now we've seen everything that we
need to send a request now we're going
to look at how the application is going
to answer with a
response there are three main components
to an HTTP response the status code the
header and the
body the status code is a three-digit
number which will give you information
on whether the request was successful or
unsuccessful the most common status
codes are going to be 200 which means
okay this is the standard successful
response it means your API request was
executed well and the application is
telling you
congrats you did a good
request 401 which means unauthorized
this is usually an authentication
problem this is the application telling
you the request that you sent me does
not contain the authentication
information necessary to make this
request this means you need to go back
and check your credential and how you
sent them or the access that your
credentials
have another one is 44 you might not
remember this from the web not found
usually this is a problem with the URL
and means that
the page or data you are looking for is
not on the URL that you indicated
finally we have 500 which is internal
server error which indicates that the
server had an error um this means it was
not your fault but the server's Fault In
general the very easy way to remember um
and understand these codes if the status
code starts with a two congrats it was a
successful HTTP request if it starts
with a four you made some kind of error
and you need to modify your request 500
means was the servers error most of the
time it just means try again later
then the same way we included a header
in the request the application is going
to send a header in the response giving
more context or detail so some common uh
response headers are going to be how
long is the content so how much content
is in the response what type of content
is it or when does this content expire
um so when how long am I going to have
access access to it
for finally sometimes we're going to
have a body the body is the actual data
being returned it can be in many
different formats it can be HTML if
we're browsing the web it can be Json or
it can be any other forms of data for
example binary
data then let's quickly talk about web
hooks web hooks or reverse apis so
imagine you're at home waiting for some
friends you can either go and check the
door every few minutes to see if they've
arrived or you can wait for the doorbell
to ring the doorbell in this analogy is
a web hook it is what indicates that
something that you are waiting for
happened so let's say you're using
stripe uh stripe is a developer platform
to manage payments and you need to know
every time a new payment is made in your
stripe account well there are two
options either we can do what is called
polling so every few minutes for example
we can make a new request to stripe a
new API request saying is there a new
payment if yes do some actions if no
wait and every few minutes keep asking
and keep polling the stripe application
to see if we have a new payment or we
can say up a web hook and every time
that a new payment is made in stripe a
web Hook is going to be sent to us
synchronously um giving us the
information on the payment so every time
we're waiting for something to happen or
we're waiting for information from a
specific application or
service sometimes these applications are
going to allow us to create web hooks
what we need to set up a web Hook is a
URL is what is the location we are
sending the information to and then we
can use a tool like nadn and the web
hook node to receive and manage and deal
with this information triggering a
workflow that was it for the second
video uh of this beginner course on apis
and web hooks in the next video we will
be covering anded end nodes and
everything you need to know to start
building your first workflows see you in
the next video