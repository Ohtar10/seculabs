# certs

This directory contains dummy openssl certificates and instructions in how to generate them.

This is purely for reference.

## Root CA
This is the certificate of origin, it should be used only for generating other certificates that authorizes
other components. This could be the equivalent of any well-known Certificate provider like Verisign, Digicert, Glibalsign, etc.
Any identity verification check with the certificate will follow a certificate chain that can be traced to the Root CA, being
this, a valid authority that verifies your identity as a holder of for example a domain.

Look inside rootCA folder for instructions on how to generate them.