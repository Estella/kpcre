# kpcre2

PCRE linux kernel module &amp; PCRE based text search engine

### PCRE library kernel module (libpcre2-X.ko)

The PCRE library is a set of functions that implement regular expression pattern matching using the same syntax and semantics as Perl 5.
PCRE has its own native API, as well as a set of wrapper functions that correspond to the POSIX regular expression API. 

This is a PCRE2 library ported to linux kernel.
It is a full-fledged PCRE kernel module which supports JIT mode.

Original sources: ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre

### PCRE based text search engine (ts_pcre.ko)

This is a new text search engine based on the PCRE kernel module.

### Installing

The install the modules follow these steps, always from the modules package root (i.e. where this file is located)

To compile the modules first type:

```
make modules
```

Then, as root type:

```
make modules_install
```

That would install the pcre & ts_pcre modules for the given Linux kernel.

### Usage

Once you have installed both modules ("libpcre2-X.ko" and "ts_pcre.ko")
you should type (as root):


modprobe ts_pcre


If the module has been successfully loaded you shouldn't see any message.
After loading the kernel module you can use iptables to add a rule.

An example rule would be (as root):


iptables -A INPUT -m string --string "/\x7C\x7C.+[a-z]/i" --algo pcre -j DROP


This wouldn't allow any incoming traffic that has the content matching the given PCRE in the payload.

In case you want to stop using the ts_pcre kernel module, first remove every iptables rule for ts_pcre and then type (as root):


modprobe -r ts_pcre


You can also use PCRE library functions anywhere inside the kernel.
