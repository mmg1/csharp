# csharp
A place for me to store some C# tooling for red team/pentesting efforts.

### ExecutionTesting.cs
Execute process under a different PID and retrieve the output.    
Usage: `ExecutionTesting.exe <pid>`

I've been using this for a while now within a C2 framework with some minor changes and has been pretty stable. This PoC will execute a command under a specified PID. You will need proper permissions on the process to launch a child under it. Sound familiar? Cobalt Strike introduced this feature [here](https://blog.cobaltstrike.com/2017/05/23/cobalt-strike-3-8-whos-your-daddy/) which also referenced Didier Stevens' [blog](https://blog.didierstevens.com/2017/03/20/that-is-not-my-child-process/) who found this back in 2009!

I *believe* this is the first public example of actually retrieving output from a process executed with another PID (Cobalt Strike can do it also). To achieve this I had to create a pipe with CreatePipe(), then use DuplicateHandle() to send a handle to the selected parent PID, then the new process should inherit that handle (due to STARTF_USESTDHANDLES) for the pipe and send stdout to the pipe. Our original process will then poll and read from that pipe. Would love to hear of any alternatives or better ways to achieve!

Update 11-14-2019: Added mitigation policy to block non-Microsoft signed DLLs. This is similar to Cobalt Strike's `blockdlls` feature and was ported in from referencing [@xpn](https://twitter.com/_xpn_)'s great work which can be found [here](https://blog.xpnsec.com/protecting-your-malware/).

### PPID.task
This is a modified code from ExecutionTesting.cs which is compatible with Covenant Tasks system. 

More information about PPID.task is in my blog
