ovftool can use a 'sessionticket'. In powershell there are enough
examples, but not for linux.

Using the perl sdk we can retrieve a session ticket, it seems:

https://github.com/lamw/vghetto-scripts/blob/master/perl/generateHTML5VMConsole.pl#L50

After acquiring the session ticket, we should then feed it to ovftool
just like the powershell scripts do:

ovftool --sourceType="VI" --I:sourceSessionTicket=$herethevar $source $target
