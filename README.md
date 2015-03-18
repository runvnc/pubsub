# Simplest pubsub

Commands sent over TCP: (pub)lish, (sub)scribe, (unsub)scribe.
Messages are not forwarded by default although there is a convention for
this using names starting with /fwd/ (see below).

```pub <name> <length> <data>
sub <pcre>
unsub <pcre>```

Names cannot contain spaces.   Subscribe and unsubscribe to groups of
messages using PCREs (Perl Compatible Regular Expressions).

Example:

```
sub myorg/chat/*
pub myorg/chat/main 5 hello
```

The most compatible convention is to specify names as paths using
`/`, but this is not a requirement.  Names can be anything as long as they
don't contain spaces.  Other than no spaces, aplications may create 
their own conventions for names.

For many applications with limited networks the above will be adequate.

# Message/Data Forwarding Convention

In cases where complex mesh networks are necessary rather than having 
all peers directly connected, a simple convention for subscribing to
forwarded data looks like this:

```
sub /fwd/myorg/chat/*
```

# Peer Discovery

Example:

```
sub /peers/myorg/*
sub /fwd/peers/myorg/*
```

Publishing peer data:

```
pub /peers/myorg/bob 12 1.2.3.4 9014
```


