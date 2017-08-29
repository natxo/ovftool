ovftool can use a 'sessionticket'. In powershell there are enough
examples, but not for linux.

Using the perl sdk we can retrieve a session ticket, it seems:

https://github.com/lamw/vghetto-scripts/blob/master/perl/generateHTML5VMConsole.pl#L50


And it works :-)

Running this will promt you for your user name and password, and will
print the session ticket to the screen. This ticket is valid for
5 minutes or until you disconnect the session with ctrl-c on your shell.


~~~~
use strict;
use warnings;

use VMware::VILib;
use VMware::VIRuntime;

Opts::parse();
Opts::validate();
Util::connect();

my $sessionMgr =
  Vim::get_view( mo_ref => Vim::get_service_content()->sessionManager );
my $session = $sessionMgr->AcquireCloneTicket();

print "session ticket:\n";
print $session;
print "\n";
print "Sleeping for 300 seconds before disconnecting the session...\n";

sleep(300);

~~~~

After acquiring the session ticket, we should then feed it to ovftool
just like the powershell scripts do:


`ovftool --sourceType="VI" --I:sourceSessionTicket="paste here the ticket" vi://esxhost`

will get you the list of vms on the host withouth exposing yet another
password on the cli. This ticket is valid for just one session, so if
you need several ovftool commands you will need to retrieve a ticket for
each one.

