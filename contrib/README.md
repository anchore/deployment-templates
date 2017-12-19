# Contrib Directory

The purpose of the _contrib/_ directory is to make clear which deployment configurations are regularly tested by the anchore team itself and which ones should work but are not part of our regular testing processes.

Specifically, things like alternative volume mappings, use of other ways to distribute or map docker resources into the containers are also good candidates for this repository.

If we are able commit to regular testing of a particular configuration it may be moved from _contrib/_ into the appropriate directory to indicate the change in tested status.
