# Simplest pubsub

This simple protocol can be used for both realtime communication as well
as document or file distribution.  Applications create their own 
conventions tailoring for specific uses while adhering to the basic 
protocol involving just three commands.

Commands are sent over TCP: (pub)lish, (sub)scribe, (unsub)scribe.
Messages are not forwarded by default although there is a convention for
this using names starting with //fwd/ (see below).

```
pub <name> <length> <data><LF>
sub <substring><LF>
unsub <substring><LF>
```

Names cannot contain spaces or CR or LF.  Commands, names and text 
data are encoded in UTF-8.  Data may be binary depending on the
application.

Subscribe and unsubscribe to groups of messages by specifying 
text that the names must contain.

Example:

```
sub //myorg/chat/
pub //myorg/chat/main 5 hello
```

In the example above we subscribe to all messages with names containing 
the text `//myorg/chat/` and then publish a `hello` message to that
name.  In this application names represent simple message channels; 
other application may adopt a convention requiring data to have unique
ids or have other constraints/schemes. 

The most compatible convention is to specify names as paths using
`/`, but this is not a requirement.  Names can be anything as long as they
don't contain spaces or CR or LF.

Note the space after the length and before the data starts in `pub`.

The basic flow works like this:

* Connect to one or more peers.

* Send `sub` commands indicating desired data or message channels.  

* Receive `sub` and `unsub` commands and record subscriptions.

* To publish: for each peer, check if the name contains the text of one 
  of that peer's subscriptions and if it does, send them the `pub`
  command.

* Receive `pub` commands with requested data/updates.

For many applications with limited networks the above will be adequate.

# Message/Data Forwarding Convention

In cases where complex mesh networks are necessary rather than having 
all peers directly connected, a simple convention for subscribing to
forwarded data looks like this:

```
sub //fwd/myorg/chat/
```

Upon receiving `//myorg/chat` data, publish a copy to `//fwd/myorg/chat`. 

# Peer Discovery Example

Example:

```
sub //peers/myorg/
sub //fwd/peers/myorg/
```

Publishing peer data:

```
pub //peers/myorg/bob 12 1.2.3.4 9014
```

# Overlay Networks

Applications may choose their own systems for creating
peer networks and ensuring efficient, reliable, and secure delivery
of messages, so long as they use the three commands as specified.

